# Useful Links - FastAPI & API Development

**Topic 7**: API Development with FastAPI
**Resources**: FastAPI, REST APIs, Pydantic, testing

---

## 📚 Official Documentation

### FastAPI Core
- **[FastAPI Documentation](https://fastapi.tiangolo.com/)** - Complete FastAPI reference
- **[FastAPI Tutorial](https://fastapi.tiangolo.com/tutorial/)** - Step-by-step FastAPI guide
- **[FastAPI Advanced](https://fastapi.tiangolo.com/advanced/)** - Advanced FastAPI features

### Pydantic
- **[Pydantic Documentation](https://pydantic-docs.helpmanual.io/)** - Data validation library
- **[Pydantic Models](https://pydantic-docs.helpmanual.io/usage/models/)** - Model creation and validation
- **[Pydantic Validators](https://pydantic-docs.helpmanual.io/usage/validators/)** - Custom validation rules

### Uvicorn
- **[Uvicorn Documentation](https://www.uvicorn.org/)** - ASGI server for FastAPI
- **[Uvicorn Settings](https://www.uvicorn.org/settings/)** - Server configuration options
- **[Deployment](https://www.uvicorn.org/deployment/)** - Production deployment guides

---

## 🌐 REST API Design

### REST Principles
- **[REST API Tutorial](https://restfulapi.net/)** - Complete REST API guide
- **[RESTful Web Services](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)** - Roy Fielding's dissertation
- **[HTTP Status Codes](https://httpstatuses.com/)** - Complete HTTP status code reference

### API Design Best Practices
- **[API Design Guide](https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design)** - Microsoft API design guide
- **[JSON API](https://jsonapi.org/)** - Standard for JSON API responses
- **[OpenAPI Specification](https://swagger.io/specification/)** - API documentation standard

### API Versioning
- **[API Versioning](https://www.xmatters.com/blog/blog/2017/06/26/api-versioning)** - API versioning strategies
- **[URL Versioning](https://restfulapi.net/versioning/)** - Different versioning approaches
- **[Semantic Versioning](https://semver.org/)** - Version numbering standard

---

## 🧪 API Testing & Validation

### FastAPI Testing
- **[FastAPI Testing](https://fastapi.tiangolo.com/tutorial/testing/)** - Official FastAPI testing guide
- **[TestClient](https://fastapi.tiangolo.com/tutorial/testing/#use-testclient)** - FastAPI's test client
- **[Async Testing](https://fastapi.tiangolo.com/tutorial/testing/#async-tests)** - Testing async endpoints

### HTTP Testing Tools
- **[HTTPie](https://httpie.io/)** - User-friendly HTTP client
- **[curl](https://curl.se/docs/manpage.html)** - Command-line HTTP client
- **[Postman](https://www.postman.com/)** - GUI API testing tool
- **[Insomnia](https://insomnia.rest/)** - REST client with GraphQL support

### Testing Libraries
- **[pytest](https://docs.pytest.org/)** - Python testing framework
- **[pytest-asyncio](https://pytest-asyncio.readthedocs.io/)** - Async testing support
- **[httpx](https://www.python-httpx.org/)** - Async HTTP client for testing
- **[requests-mock](https://requests-mock.readthedocs.io/)** - Mock HTTP requests

---

## 🗄️ Databases & ORMs

### SQLAlchemy (Recommended for FastAPI)
- **[SQLAlchemy Documentation](https://sqlalchemy.org/)** - Python SQL toolkit
- **[SQLAlchemy ORM](https://docs.sqlalchemy.org/en/14/orm/)** - Object-relational mapping
- **[FastAPI SQLAlchemy](https://fastapi.tiangolo.com/tutorial/sql-databases/)** - FastAPI with SQLAlchemy

### Alternative ORMs
- **[Tortoise ORM](https://tortoise.github.io/)** - Async ORM for FastAPI
- **[Peewee](http://docs.peewee-orm.com/)** - Simple ORM
- **[Django ORM](https://docs.djangoproject.com/en/stable/topics/db/)** - Django's ORM (with Django Ninja)

### Database Drivers
- **[asyncpg](https://magicstack.github.io/asyncpg/)** - Async PostgreSQL driver
- **[aiomysql](https://aiomysql.readthedocs.io/)** - Async MySQL driver
- **[aiosqlite](https://aiosqlite.omniliberal.org/)** - Async SQLite driver

---

## 🔐 Authentication & Security

### FastAPI Security
- **[FastAPI Security](https://fastapi.tiangolo.com/tutorial/security/)** - Official security guide
- **[OAuth2](https://fastapi.tiangolo.com/tutorial/security/oauth2-jwt/)** - JWT token authentication
- **[API Keys](https://fastapi.tiangolo.com/tutorial/security/api-key/)** - API key authentication

### Security Libraries
- **[python-jose](https://python-jose.readthedocs.io/)** - JWT encoding/decoding
- **[passlib](https://passlib.readthedocs.io/)** - Password hashing
- **[bcrypt](https://pypi.org/project/bcrypt/)** - Secure password hashing
- **[python-multipart](https://pypi.org/project/python-multipart/)** - File upload security

### Security Best Practices
- **[OWASP API Security](https://owasp.org/www-project-api-security/)** - API security guidelines
- **[JWT Best Practices](https://tools.ietf.org/html/rfc8725)** - JSON Web Token security
- **[Rate Limiting](https://fastapi.tiangolo.com/tutorial/middleware/#rate-limiting)** - API rate limiting

---

## 📊 Data Validation & Serialization

### Advanced Pydantic
- **[Pydantic Settings](https://pydantic-docs.helpmanual.io/usage/settings/)** - Application settings management
- **[Pydantic Export](https://pydantic-docs.helpmanual.io/usage/exporting_models/)** - Model serialization
- **[Custom Types](https://pydantic-docs.helpmanual.io/usage/types/)** - Custom field types

### Alternative Validation
- **[marshmallow](https://marshmallow.readthedocs.io/)** - Object serialization library
- **[Cerberus](https://docs.python-cerberus.org/)** - Validation library
- **[schema](https://github.com/keleshev/schema)** - Simple validation library

### Data Processing
- **[pandas](https://pandas.pydata.org/)** - Data manipulation and analysis
- **[orjson](https://github.com/ijl/orjson)** - Fast JSON processing
- **[ujson](https://pypi.org/project/ujson/)** - Ultra-fast JSON parsing

---

## ⚡ Performance & Scaling

### Async Programming
- **[AsyncIO Documentation](https://docs.python.org/3/library/asyncio.html)** - Python async programming
- **[FastAPI Async](https://fastapi.tiangolo.com/async/)** - Async endpoints in FastAPI
- **[Concurrent Programming](https://realpython.com/async-io-python/)** - Async best practices

### Caching
- **[Redis](https://redis.io/)** - In-memory data store
- **[aioredis](https://aioredis.readthedocs.io/)** - Async Redis client
- **[FastAPI Cache](https://github.com/long2ice/fastapi-cache)** - Caching for FastAPI

### Background Tasks
- **[FastAPI Background Tasks](https://fastapi.tiangolo.com/tutorial/background-tasks/)** - Background task processing
- **[Celery](https://docs.celeryproject.org/)** - Distributed task queue
- **[RQ (Redis Queue)](https://python-rq.org/)** - Simple job queues

---

## 📱 API Clients & SDKs

### Python API Clients
- **[httpx](https://www.python-httpx.org/)** - Async HTTP client
- **[aiohttp](https://docs.aiohttp.org/)** - Async HTTP client/server
- **[requests](https://requests.readthedocs.io/)** - Synchronous HTTP library

### API Documentation
- **[Swagger UI](https://swagger.io/tools/swagger-ui/)** - Interactive API docs (built into FastAPI)
- **[ReDoc](https://github.com/Redocly/redoc)** - Alternative API documentation (built into FastAPI)
- **[Stoplight](https://stoplight.io/)** - API design and documentation platform

### SDK Generation
- **[OpenAPI Generator](https://openapi-generator.tech/)** - Generate API clients
- **[dataclasses-avroschema](https://github.com/marcosschroh/dataclasses-avroschema)** - Avro schema generation
- **[pydantic-to-typescript](https://github.com/phillipdupuis/pydantic-to-typescript)** - TypeScript types from Pydantic

---

## 🏗️ Deployment & Production

### ASGI Servers
- **[Gunicorn](https://gunicorn.org/)** - WSGI HTTP Server (with Uvicorn workers)
- **[Hypercorn](https://pgjones.gitlab.io/hypercorn/)** - ASGI server
- **[Daphne](https://github.com/django/daphne)** - Django Channels ASGI server

### Cloud Deployment
- **[Railway](https://railway.app/)** - FastAPI deployment platform
- **[Render](https://render.com/)** - Cloud application hosting
- **[Heroku](https://devcenter.heroku.com/articles/getting-started-with-python)** - Heroku Python deployment
- **[DigitalOcean App Platform](https://www.digitalocean.com/products/app-platform/)** - Cloud platform

### Containerization
- **[Docker FastAPI](https://fastapi.tiangolo.com/deployment/docker/)** - Docker deployment guide
- **[Kubernetes](https://kubernetes.io/)** - Container orchestration
- **[Docker Compose](https://docs.docker.com/compose/)** - Multi-container applications

---

## 🎨 Advanced FastAPI Features

### Dependency Injection
- **[FastAPI Dependencies](https://fastapi.tiangolo.com/tutorial/dependencies/)** - Dependency injection system
- **[Sub-dependencies](https://fastapi.tiangolo.com/tutorial/dependencies/sub-dependencies/)** - Complex dependency chains
- **[Global Dependencies](https://fastapi.tiangolo.com/tutorial/dependencies/global-dependencies/)** - Application-wide dependencies

### Middleware
- **[FastAPI Middleware](https://fastapi.tiangolo.com/tutorial/middleware/)** - Custom middleware
- **[CORS](https://fastapi.tiangolo.com/tutorial/cors/)** - Cross-origin resource sharing
- **[Custom Middleware](https://fastapi.tiangolo.com/advanced/middleware/)** - Advanced middleware patterns

### WebSockets
- **[FastAPI WebSockets](https://fastapi.tiangolo.com/advanced/websockets/)** - WebSocket support
- **[WebSocket Testing](https://fastapi.tiangolo.com/advanced/testing-websockets/)** - Testing WebSocket connections

### File Handling
- **[File Uploads](https://fastapi.tiangolo.com/tutorial/request-files/)** - File upload handling
- **[File Responses](https://fastapi.tiangolo.com/advanced/custom-response/#streamingresponse)** - File download responses
- **[Static Files](https://fastapi.tiangolo.com/tutorial/static-files/)** - Serving static files

---

## 🧪 Testing Strategies

### Unit Testing
- **[pytest Fixtures](https://docs.pytest.org/en/stable/fixture.html)** - Test fixtures and setup
- **[pytest Parametrize](https://docs.pytest.org/en/stable/parametrize.html)** - Parameterized tests
- **[pytest-mock](https://pytest-mock.readthedocs.io/)** - Mocking in pytest

### Integration Testing
- **[TestClient](https://fastapi.tiangolo.com/tutorial/testing/#use-testclient)** - FastAPI test client
- **[Test Database](https://fastapi.tiangolo.com/tutorial/testing-database/)** - Testing with databases
- **[Async Tests](https://fastapi.tiangolo.com/tutorial/testing/#async-tests)** - Testing async endpoints

### Load Testing
- **[Locust](https://docs.locust.io/)** - Load testing framework
- **[k6](https://k6.io/)** - Modern load testing
- **[Artillery](https://artillery.io/)** - Load testing and functional testing

---

## 📖 Books and Courses

### Recommended Books
- **[FastAPI Book](https://fastapi.tiangolo.com/tutorial/)** - Official FastAPI tutorial (free)
- **[Building APIs with FastAPI](https://www.packtpub.com/product/building-apis-with-fastapi/9781801076630)** - Comprehensive FastAPI guide
- **[REST API Design Rulebook](https://www.oreilly.com/library/view/rest-api-design/9781449317904/)** - API design principles

### Online Courses
- **[FastAPI Course](https://fastapi.tiangolo.com/tutorial/)** - Official FastAPI tutorial
- **[REST API with FastAPI](https://www.udemy.com/course/rest-api-with-fastapi/)** - Complete FastAPI course
- **[API Design](https://www.coursera.org/learn/api-design)** - API design principles

### Video Resources
- **[FastAPI YouTube](https://www.youtube.com/results?search_query=fastapi+tutorial)** - FastAPI tutorial videos
- **[API Design Videos](https://www.youtube.com/results?search_query=rest+api+design)** - API design tutorials
- **[Pydantic Tutorials](https://www.youtube.com/results?search_query=pydantic+tutorial)** - Pydantic video guides

---

## 🌐 Community Resources

### Forums and Communities
- **[FastAPI Discussions](https://github.com/tiangolo/fastapi/discussions)** - Official FastAPI community
- **[FastAPI Reddit](https://www.reddit.com/r/FastAPI/)** - FastAPI community on Reddit
- **[Python API Development](https://stackoverflow.com/questions/tagged/fastapi)** - Stack Overflow Q&A

### Blogs and Newsletters
- **[FastAPI Blog](https://dev.to/t/fastapi)** - FastAPI articles on Dev.to
- **[Real Python APIs](https://realpython.com/tutorials/api/)** - API development tutorials
- **[Pycoder's Weekly](https://pycoders.com/)** - Python news including API developments

### Social Media
- **[FastAPI Twitter](https://twitter.com/fastapi)** - Official FastAPI Twitter
- **[Python API Twitter](https://twitter.com/search?q=%23FastAPI)** - FastAPI hashtag on Twitter
- **[API Design LinkedIn](https://www.linkedin.com/search/results/content/?keywords=api%20design)** - API design discussions

---

## 🛠️ Development Tools

### API Development Tools
- **[Insomnia Designer](https://insomnia.rest/products/designer/)** - API design and documentation
- **[Stoplight Studio](https://stoplight.io/studio)** - API design and mocking
- **[Postman Collections](https://learning.postman.com/docs/sending-requests/intro-to-collections/)** - API testing collections

### Code Quality
- **[Black](https://black.readthedocs.io/)** - Code formatting
- **[isort](https://pycqa.github.io/isort/)** - Import sorting
- **[mypy](https://mypy.readthedocs.io/)** - Type checking
- **[flake8](https://flake8.pycqa.org/)** - Linting

### Monitoring
- **[Sentry](https://sentry.io/)** - Error tracking and monitoring
- **[New Relic](https://newrelic.com/)** - Application performance monitoring
- **[DataDog APM](https://docs.datadoghq.com/tracing/)** - Distributed tracing

---

## 🎯 API Standards and Specifications

### API Specifications
- **[OpenAPI 3.0](https://swagger.io/specification/)** - API description format
- **[JSON Schema](https://json-schema.org/)** - JSON data validation
- **[AsyncAPI](https://www.asyncapi.com/)** - Event-driven API specification

### Industry Standards
- **[RFC 7231](https://tools.ietf.org/html/rfc7231)** - HTTP/1.1 semantics and content
- **[RFC 8259](https://tools.ietf.org/html/rfc8259)** - JSON data interchange format
- **[RFC 8725](https://tools.ietf.org/html/rfc8725)** - JSON Web Token best practices

### Compliance
- **[GDPR API](https://gdpr.eu/)** - Privacy-compliant API design
- **[WCAG](https://www.w3.org/WAI/WCAG21/quickref/)** - Web accessibility guidelines
- **[OWASP API](https://owasp.org/www-project-api-security/)** - API security guidelines

---

## 🚀 Production Deployment

### Cloud Platforms
- **[AWS Lambda](https://aws.amazon.com/lambda/)** - Serverless FastAPI deployment
- **[Google Cloud Run](https://cloud.google.com/run)** - Containerized FastAPI
- **[Azure Functions](https://azure.microsoft.com/en-us/services/functions/)** - Serverless computing
- **[Vercel](https://vercel.com/)** - Frontend-focused deployment

### Infrastructure as Code
- **[Terraform](https://www.terraform.io/)** - Infrastructure provisioning
- **[AWS CDK](https://aws.amazon.com/cdk/)** - Infrastructure as code
- **[Pulumi](https://www.pulumi.com/)** - Modern infrastructure as code

### CI/CD for APIs
- **[GitHub Actions FastAPI](https://github.com/marketplace/actions/setup-python)** - FastAPI CI/CD
- **[Azure DevOps](https://azure.microsoft.com/en-us/services/devops/)** - Complete DevOps platform
- **[GitLab CI](https://docs.gitlab.com/ee/ci/)** - GitLab's CI/CD platform

---

**Last Updated**: February 2026
**Total Resources**: 100+ links
**Categories**: Documentation, Frameworks, Testing, Deployment, Community