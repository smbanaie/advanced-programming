# Homework 2: UV Package Manager Project

**Topic**: 2 - Python Environment & Dependency Management
**Section**: 2 of 2 sections (90 min workshop + homework)
**Level**: Intermediate
**Prerequisites**: Section 1 (Virtual Environments), UV installation

---

## 🎯 Learning Objectives

After completing this homework, you will demonstrate the ability to:

1. **Use UV Package Manager**: Create and manage projects with UV
2. **Handle Complex Dependencies**: Manage multiple packages with version constraints
3. **Build Complete Applications**: Develop a functional application with proper structure
4. **Deploy with UV**: Use UV for running and deploying applications

---

## 📋 Assignment Overview

Create a complete Python application using UV for dependency management and project structure. The application should demonstrate modern Python development practices.

### Project Requirements

**Application**: Weather Dashboard CLI Tool

Features:
- Fetch weather data from OpenWeatherMap API
- Display current weather and forecast
- Cache data to avoid excessive API calls
- Command-line interface with multiple options
- Configuration management

### Technical Requirements

1. **Project Structure** (using UV):
   ```
   weather-dashboard/
   ├── pyproject.toml          # UV project configuration
   ├── src/
   │   └── weather_dashboard/
   │       ├── __init__.py
   │       ├── cli.py          # Command-line interface
   │       ├── api.py          # Weather API client
   │       ├── cache.py        # Data caching
   │       ├── config.py       # Configuration management
   │       └── models.py       # Data models
   ├── tests/
   │   ├── test_api.py
   │   ├── test_cache.py
   │   └── test_cli.py
   └── README.md
   ```

2. **Dependencies to Manage**:
   - `requests` for HTTP requests
   - `click` for CLI interface
   - `pydantic` for data validation
   - `python-dotenv` for environment variables
   - `pytest` for testing (dev dependency)

---

## 🛠️ Step-by-Step Implementation

### Step 1: Project Initialization with UV

```bash
# Create new UV project
uv init weather-dashboard

# Change to project directory
cd weather-dashboard

# Add dependencies
uv add requests click pydantic python-dotenv
uv add --dev pytest

# Verify project structure
tree src/
```

### Step 2: Configuration Management

Create `src/weather_dashboard/config.py`:

```python
"""Configuration management for Weather Dashboard."""

import os
from typing import Optional
from pydantic import BaseSettings, Field


class Settings(BaseSettings):
    """Application settings with validation."""

    # API Configuration
    openweather_api_key: str = Field(..., env="OPENWEATHER_API_KEY")
    api_base_url: str = "https://api.openweathermap.org/data/2.5"

    # Cache Configuration
    cache_ttl_seconds: int = Field(default=300, gt=0)  # 5 minutes
    cache_dir: str = Field(default=".cache")

    # Application Configuration
    default_city: str = Field(default="London")
    temperature_unit: str = Field(default="celsius", regex="^(celsius|fahrenheit|kelvin)$")

    class Config:
        env_file = ".env"
        case_sensitive = False


# Global settings instance
settings = Settings()
```

### Step 3: Data Models

Create `src/weather_dashboard/models.py`:

```python
"""Data models for weather information."""

from typing import List, Optional
from pydantic import BaseModel
from datetime import datetime


class WeatherCondition(BaseModel):
    """Weather condition information."""
    id: int
    main: str
    description: str
    icon: str


class MainWeatherData(BaseModel):
    """Main weather metrics."""
    temp: float
    feels_like: float
    temp_min: float
    temp_max: float
    pressure: int
    humidity: int


class WindData(BaseModel):
    """Wind information."""
    speed: float
    deg: Optional[int] = None


class WeatherResponse(BaseModel):
    """Complete weather API response."""
    weather: List[WeatherCondition]
    main: MainWeatherData
    wind: WindData
    name: str
    dt: int  # Unix timestamp

    @property
    def temperature_celsius(self) -> float:
        """Get temperature in Celsius."""
        return self.main.temp - 273.15

    @property
    def temperature_fahrenheit(self) -> float:
        """Get temperature in Fahrenheit."""
        return (self.main.temp - 273.15) * 9/5 + 32


class ForecastItem(BaseModel):
    """Individual forecast item."""
    dt: int
    main: MainWeatherData
    weather: List[WeatherCondition]
    dt_txt: str


class ForecastResponse(BaseModel):
    """Weather forecast API response."""
    list: List[ForecastItem]
    city: dict
```

### Step 4: API Client

Create `src/weather_dashboard/api.py`:

```python
"""Weather API client."""

import requests
from typing import Optional, Union
from .config import settings
from .models import WeatherResponse, ForecastResponse


class WeatherAPI:
    """Client for OpenWeatherMap API."""

    def __init__(self, api_key: Optional[str] = None):
        self.api_key = api_key or settings.openweather_api_key
        self.base_url = settings.api_base_url

    def get_current_weather(self, city: str) -> WeatherResponse:
        """
        Get current weather for a city.

        Args:
            city: City name

        Returns:
            WeatherResponse object

        Raises:
            requests.HTTPError: If API request fails
        """
        params = {
            'q': city,
            'appid': self.api_key,
            'units': 'metric'
        }

        response = requests.get(f"{self.base_url}/weather", params=params, timeout=10)
        response.raise_for_status()

        return WeatherResponse(**response.json())

    def get_weather_forecast(self, city: str, days: int = 5) -> ForecastResponse:
        """
        Get weather forecast for a city.

        Args:
            city: City name
            days: Number of days (max 5)

        Returns:
            ForecastResponse object
        """
        params = {
            'q': city,
            'appid': self.api_key,
            'units': 'metric',
            'cnt': min(days * 8, 40)  # 8 readings per day, max 40
        }

        response = requests.get(f"{self.base_url}/forecast", params=params, timeout=10)
        response.raise_for_status()

        return ForecastResponse(**response.json())
```

### Step 5: Caching System

Create `src/weather_dashboard/cache.py`:

```python
"""Simple file-based caching system."""

import json
import os
from pathlib import Path
from typing import Any, Optional
from datetime import datetime, timedelta
from .config import settings


class Cache:
    """File-based cache with TTL support."""

    def __init__(self, cache_dir: str = None):
        self.cache_dir = Path(cache_dir or settings.cache_dir)
        self.cache_dir.mkdir(exist_ok=True)

    def _get_cache_path(self, key: str) -> Path:
        """Get cache file path for a key."""
        # Simple hash for filename
        key_hash = str(hash(key) % 1000000)
        return self.cache_dir / f"{key_hash}.json"

    def get(self, key: str) -> Optional[Any]:
        """
        Get value from cache if not expired.

        Args:
            key: Cache key

        Returns:
            Cached value or None if expired/missing
        """
        cache_path = self._get_cache_path(key)

        if not cache_path.exists():
            return None

        try:
            with open(cache_path, 'r') as f:
                data = json.load(f)

            # Check TTL
            cached_time = datetime.fromisoformat(data['timestamp'])
            if datetime.now() - cached_time > timedelta(seconds=settings.cache_ttl_seconds):
                cache_path.unlink()  # Remove expired cache
                return None

            return data['value']

        except (json.JSONDecodeError, KeyError, ValueError):
            # Invalid cache file, remove it
            cache_path.unlink()
            return None

    def set(self, key: str, value: Any) -> None:
        """
        Store value in cache with timestamp.

        Args:
            key: Cache key
            value: Value to cache
        """
        cache_path = self._get_cache_path(key)

        data = {
            'timestamp': datetime.now().isoformat(),
            'value': value
        }

        with open(cache_path, 'w') as f:
            json.dump(data, f, indent=2)

    def clear(self) -> None:
        """Clear all cached data."""
        for cache_file in self.cache_dir.glob("*.json"):
            cache_file.unlink()
```

### Step 6: Command-Line Interface

Create `src/weather_dashboard/cli.py`:

```python
"""Command-line interface for Weather Dashboard."""

import click
from .api import WeatherAPI
from .cache import Cache
from .config import settings


@click.group()
@click.option('--city', default=None, help='City name (overrides default)')
@click.option('--unit', type=click.Choice(['celsius', 'fahrenheit']),
              default=None, help='Temperature unit')
@click.pass_context
def cli(ctx, city, unit):
    """Weather Dashboard - Get weather information from command line."""
    # Store shared objects in context
    ctx.ensure_object(dict)
    ctx.obj['api'] = WeatherAPI()
    ctx.obj['cache'] = Cache()
    ctx.obj['city'] = city or settings.default_city
    ctx.obj['unit'] = unit or settings.temperature_unit


@cli.command()
@click.pass_context
def current(ctx):
    """Show current weather for a city."""
    api = ctx.obj['api']
    cache = ctx.obj['cache']
    city = ctx.obj['city']
    unit = ctx.obj['unit']

    # Try cache first
    cache_key = f"current_{city}"
    weather_data = cache.get(cache_key)

    if weather_data is None:
        click.echo(f"Fetching current weather for {city}...")
        try:
            weather = api.get_current_weather(city)
            weather_data = weather.dict()
            cache.set(cache_key, weather_data)
        except Exception as e:
            click.echo(f"Error: {e}", err=True)
            return
    else:
        click.echo(f"Using cached data for {city}")
        weather = WeatherResponse(**weather_data)

    # Display weather information
    temp = weather.temperature_celsius if unit == 'celsius' else weather.temperature_fahrenheit
    unit_symbol = '°C' if unit == 'celsius' else '°F'

    click.echo(f"\nCurrent Weather for {weather.name}")
    click.echo("=" * 40)
    click.echo(f"Temperature: {temp:.1f}{unit_symbol}")
    click.echo(f"Feels like: {weather.main.feels_like:.1f}{unit_symbol}")
    click.echo(f"Condition: {weather.weather[0].description.title()}")
    click.echo(f"Humidity: {weather.main.humidity}%")
    click.echo(f"Wind Speed: {weather.wind.speed} m/s")


@cli.command()
@click.option('--days', default=3, type=click.IntRange(1, 5),
              help='Number of days for forecast')
@click.pass_context
def forecast(ctx, days):
    """Show weather forecast for a city."""
    api = ctx.obj['api']
    cache = ctx.obj['cache']
    city = ctx.obj['city']
    unit = ctx.obj['unit']

    cache_key = f"forecast_{city}_{days}"
    forecast_data = cache.get(cache_key)

    if forecast_data is None:
        click.echo(f"Fetching {days}-day forecast for {city}...")
        try:
            forecast = api.get_weather_forecast(city, days)
            forecast_data = forecast.dict()
            cache.set(cache_key, forecast_data)
        except Exception as e:
            click.echo(f"Error: {e}", err=True)
            return
    else:
        click.echo(f"Using cached forecast for {city}")
        forecast = ForecastResponse(**forecast_data)

    # Display forecast (simplified)
    click.echo(f"\n{days}-Day Weather Forecast for {forecast.city['name']}")
    click.echo("=" * 50)

    # Group by date
    from collections import defaultdict
    daily_forecasts = defaultdict(list)

    for item in forecast.list[:days*8]:  # 8 readings per day
        date = item.dt_txt.split()[0]
        daily_forecasts[date].append(item)

    for date, items in list(daily_forecasts.items())[:days]:
        day_temp = sum(item.main.temp for item in items) / len(items)
        temp = day_temp - 273.15 if unit == 'celsius' else (day_temp - 273.15) * 9/5 + 32
        unit_symbol = '°C' if unit == 'celsius' else '°F'

        click.echo(f"{date}: {temp:.1f}{unit_symbol} - {items[0].weather[0].description}")


@cli.command()
@click.pass_context
def clear_cache(ctx):
    """Clear cached weather data."""
    cache = ctx.obj['cache']
    cache.clear()
    click.echo("Cache cleared successfully!")


if __name__ == '__main__':
    cli()
```

### Step 7: Package Initialization

Create `src/weather_dashboard/__init__.py`:

```python
"""Weather Dashboard - A command-line weather application."""

__version__ = "0.1.0"
__author__ = "Your Name"

from .api import WeatherAPI
from .cache import Cache
from .cli import cli

__all__ = ["WeatherAPI", "Cache", "cli"]
```

### Step 8: Testing

Create basic tests in `tests/test_api.py`:

```python
"""Tests for Weather API client."""

import pytest
from unittest.mock import Mock, patch
from weather_dashboard.api import WeatherAPI


class TestWeatherAPI:
    """Test WeatherAPI class."""

    def test_init_with_api_key(self):
        """Test initialization with API key."""
        api = WeatherAPI("test_key")
        assert api.api_key == "test_key"

    @patch('requests.get')
    def test_get_current_weather_success(self, mock_get):
        """Test successful weather fetch."""
        # Mock response
        mock_response = Mock()
        mock_response.json.return_value = {
            "weather": [{"id": 800, "main": "Clear", "description": "clear sky", "icon": "01d"}],
            "main": {"temp": 293.15, "feels_like": 293.15, "temp_min": 293.15, "temp_max": 293.15, "pressure": 1013, "humidity": 50},
            "wind": {"speed": 3.5},
            "name": "London",
            "dt": 1640995200
        }
        mock_response.raise_for_status.return_value = None
        mock_get.return_value = mock_response

        api = WeatherAPI("test_key")
        result = api.get_current_weather("London")

        assert result.name == "London"
        assert result.main.temp == 293.15
        mock_get.assert_called_once()
```

### Step 9: Project Configuration

Update `pyproject.toml` (created by UV):

```toml
[project]
name = "weather-dashboard"
version = "0.1.0"
description = "A command-line weather dashboard application"
readme = "README.md"
requires-python = ">=3.8"
dependencies = [
    "requests>=2.28.0",
    "click>=8.0.0",
    "pydantic>=1.8.0",
    "python-dotenv>=0.19.0"
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.build.targets.wheel]
packages = ["src/weather_dashboard"]

[project.scripts]
weather = "weather_dashboard.cli:cli"

[tool.pytest.ini_options]
testpaths = ["tests"]
python_files = "test_*.py"
```

### Step 10: README and Setup

Create comprehensive `README.md` and `.env.example` file.

---

## 📋 Submission Requirements

### Required Deliverables
- [ ] Complete UV project structure
- [ ] Working CLI application
- [ ] Comprehensive tests
- [ ] Proper documentation
- [ ] Environment configuration

### Testing the Application

```bash
# Install dependencies
uv sync

# Set up environment
cp .env.example .env
# Edit .env with your OpenWeatherMap API key

# Run the application
uv run weather current
uv run weather forecast --days 3
uv run weather clear-cache
```

### Project Verification
- [ ] All dependencies install correctly with UV
- [ ] Application runs without errors
- [ ] Tests pass with `uv run pytest`
- [ ] CLI commands work as expected
- [ ] Caching functionality works
- [ ] Error handling is robust

---

## 🎯 Evaluation Criteria

### UV and Project Management (30%)
- [ ] Proper UV project structure
- [ ] Dependencies correctly managed
- [ ] pyproject.toml properly configured
- [ ] Virtual environment isolation

### Application Functionality (40%)
- [ ] Weather API integration works
- [ ] CLI interface is user-friendly
- [ ] Caching system functions correctly
- [ ] Error handling is comprehensive
- [ ] Configuration management works

### Code Quality (20%)
- [ ] Clean, well-documented code
- [ ] Proper type hints and validation
- [ ] Modular, maintainable structure
- [ ] PEP 8 compliance

### Testing & Documentation (10%)
- [ ] Comprehensive test coverage
- [ ] Clear documentation and README
- [ ] Working examples provided
- [ ] Professional project structure

---

## 🚀 Bonus Features

### Advanced Functionality
1. **Interactive Mode**: Add interactive city selection
2. **Weather Alerts**: Display severe weather warnings
3. **Historical Data**: Show weather trends over time
4. **Multiple Cities**: Compare weather between cities
5. **Export Options**: Export data to CSV/JSON

### Development Tools
1. **Pre-commit Hooks**: Set up code quality checks
2. **CI/CD Pipeline**: Add GitHub Actions workflow
3. **Docker Support**: Containerize the application
4. **Documentation**: Add API documentation with Sphinx

---

## 📞 Getting Help

### Common Issues
- **API Key Problems**: Verify OpenWeatherMap API key is valid
- **UV Installation**: Ensure UV is properly installed and updated
- **Import Errors**: Check that virtual environment is activated
- **Cache Issues**: Clear cache with `weather clear-cache`

### Resources
- [UV Documentation](https://github.com/astral-sh/uv)
- [OpenWeatherMap API](https://openweathermap.org/api)
- [Click Documentation](https://click.palletsprojects.com/)
- [Pydantic Documentation](https://pydantic-docs.helpmanual.io/)

---

**Homework Version**: 1.0
**Last Updated**: February 2026
**Estimated Time**: 6-8 hours
**Total Points**: 75