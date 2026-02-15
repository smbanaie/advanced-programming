# Useful Links - Python Testing & Quality Assurance

**Topic 5**: Testing & Quality Assurance
**Resources**: unittest, pytest, coverage, best practices

---

## 📚 Official Documentation

### unittest Framework
- **[unittest Documentation](https://docs.python.org/3/library/unittest.html)** - Complete Python unittest reference
- **[TestCase Class](https://docs.python.org/3/library/unittest.html#unittest.TestCase)** - TestCase class documentation
- **[Assertions](https://docs.python.org/3/library/unittest.html#assert-methods)** - Complete list of assertion methods

### Test Discovery and Running
- **[Test Discovery](https://docs.python.org/3/library/unittest.html#test-discovery)** - How Python finds and runs tests
- **[TestLoader](https://docs.python.org/3/library/unittest.html#unittest.TestLoader)** - Loading and organizing tests
- **[TextTestRunner](https://docs.python.org/3/library/unittest.html#unittest.TextTestRunner)** - Running tests with output

---

## 🧪 Testing Frameworks and Tools

### pytest Framework
- **[pytest Documentation](https://docs.pytest.org/)** - Modern Python testing framework
- **[pytest Fixtures](https://docs.pytest.org/en/stable/fixture.html)** - Reusable test setup
- **[pytest Parametrization](https://docs.pytest.org/en/stable/parametrize.html)** - Running tests with multiple inputs
- **[pytest Marks](https://docs.pytest.org/en/stable/mark.html)** - Categorizing and filtering tests

### Coverage Tools
- **[Coverage.py](https://coverage.readthedocs.io/)** - Code coverage measurement
- **[coverage report](https://coverage.readthedocs.io/en/stable/cmd.html#coverage-report)** - Generating coverage reports
- **[coverage html](https://coverage.readthedocs.io/en/stable/cmd.html#coverage-html)** - HTML coverage reports

### Mocking Libraries
- **[unittest.mock](https://docs.python.org/3/library/unittest.mock.html)** - Python's built-in mocking
- **[responses](https://github.com/getsentry/responses)** - Mocking HTTP requests
- **[freezegun](https://github.com/spulec/freezegun)** - Mocking time and dates

---

## 🎯 Testing Best Practices

### Testing Patterns
- **[AAA Pattern](https://medium.com/@pjbgf/title-testing-code-ocd-and-the-aaa-pattern-df133472cbba)** - Arrange-Act-Assert testing pattern
- **[FIRST Principles](https://agileinspired.com/test-first-principles/)** - Fast, Independent, Repeatable, Self-validating, Timely
- **[Right-BICEP](http://xunitpatterns.com/Right-BICEP.html)** - Right results, Boundary conditions, Inverse relationships, Cross-check, Error conditions, Performance

### Test Types
- **[Unit Testing](https://martinfowler.com/bliki/UnitTest.html)** - Testing individual units
- **[Integration Testing](https://martinfowler.com/bliki/IntegrationTest.html)** - Testing component interactions
- **[End-to-End Testing](https://martinfowler.com/bliki/PageObject.html)** - Testing complete workflows

### Code Quality
- **[Test Coverage](https://martinfowler.com/bliki/TestCoverage.html)** - Measuring test effectiveness
- **[Cyclomatic Complexity](https://en.wikipedia.org/wiki/Cyclomatic_complexity)** - Code complexity metrics
- **[Mutation Testing](https://en.wikipedia.org/wiki/Mutation_testing)** - Testing test quality

---

## 🧪 Advanced Testing Techniques

### Property-Based Testing
- **[Hypothesis](https://hypothesis.readthedocs.io/)** - Property-based testing for Python
- **[Property-Based Testing](https://fsharpforfunandprofit.com/posts/property-based-testing/)** - Introduction to property testing
- **[QuickCheck](https://en.wikipedia.org/wiki/QuickCheck)** - Original property-based testing

### Test-Driven Development
- **[TDD Cycle](https://martinfowler.com/bliki/TestDrivenDevelopment.html)** - Red-Green-Refactor
- **[TDD Benefits](https://www.agilealliance.org/glossary/tdd/)** - Why TDD works
- **[TDD Examples](https://www.codecademy.com/article/tdd-red-green-refactor)** - TDD in practice

### Behavior-Driven Development
- **[Behave](https://behave.readthedocs.io/)** - BDD framework for Python
- **[Gherkin](https://cucumber.io/docs/gherkin/)** - BDD syntax
- **[BDD vs TDD](https://www.agilealliance.org/glossary/bdd/)** - Understanding BDD

---

## 📊 Continuous Integration & Deployment

### CI/CD Platforms
- **[GitHub Actions](https://github.com/features/actions)** - GitHub's CI/CD platform
- **[GitLab CI](https://docs.gitlab.com/ee/ci/)** - GitLab's CI/CD
- **[Jenkins](https://www.jenkins.io/)** - Popular CI server
- **[CircleCI](https://circleci.com/)** - Cloud-based CI/CD

### CI/CD for Python
- **[Tox](https://tox.readthedocs.io/)** - Testing across Python versions
- **[GitHub Actions Python](https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-python)** - Python CI/CD workflows
- **[pre-commit](https://pre-commit.com/)** - Git hooks for code quality

### Quality Gates
- **[SonarQube](https://www.sonarsource.com/products/sonarqube/)** - Code quality platform
- **[Code Climate](https://codeclimate.com/)** - Automated code review
- **[Codacy](https://www.codacy.com/)** - Code analysis and quality

---

## 🐛 Debugging and Troubleshooting

### Test Debugging
- **[pdb](https://docs.python.org/3/library/pdb.html)** - Python debugger
- **[ipdb](https://github.com/gotcha/ipdb)** - Enhanced IPython debugger
- **[pytest debugging](https://docs.pytest.org/en/stable/usage.html#dropping-to-pdb-python-debugger-on-failures)** - pytest debugging features

### Test Flakiness
- **[Flaky Tests](https://testing.googleblog.com/2017/05/flaky-tests-at-google-and-how-we.html)** - Google's approach to flaky tests
- **[Test Isolation](https://martinfowler.com/bliki/UnitTest.html)** - Ensuring test independence
- **[Time-dependent Tests](https://www.agileconnection.com/article/how-deal-time-dependent-tests)** - Handling timing issues

### Common Testing Issues
- **[Test Dependencies](https://stackoverflow.com/questions/111221/test-code-dependency)** - Managing test dependencies
- **[Database Testing](https://realpython.com/testing-in-django-part-1-best-practices-and-examples/)** - Testing with databases
- **[Async Testing](https://docs.python.org/3/library/unittest.html#unittest.IsolatedAsyncioTestCase)** - Testing async code

---

## 📖 Books and Courses

### Recommended Books
- **[Test-Driven Development: By Example](https://www.amazon.com/Test-Driven-Development-Kent-Beck/dp/0321146530)** - Kent Beck's TDD classic
- **[Clean Code](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)** - Code quality principles
- **[xUnit Test Patterns](http://xunitpatterns.com/)** - Comprehensive testing patterns

### Online Courses
- **[Coursera: Software Testing](https://www.coursera.org/specializations/google-software-testing)** - Google's testing certification
- **[Udemy: Python Testing](https://www.udemy.com/topic/python-testing/)** - Comprehensive Python testing courses
- **[Test Automation University](https://testautomationu.applitools.com/)** - Free testing courses

### Video Resources
- **[PyCon Testing Talks](https://www.youtube.com/results?search_query=pycon+testing)** - Python conference testing talks
- **[Testing Playlist](https://www.youtube.com/playlist?list=PL-osiE80TeTt2d9bfVyTiXJA-UTHn6WwU)** - Corey's testing videos
- **[TDD Demonstration](https://www.youtube.com/watch?v=tddE2PnC8tk)** - Live TDD coding session

---

## 🌐 Community Resources

### Forums and Communities
- **[Python Testing Subreddit](https://www.reddit.com/r/Python/)** - Testing discussions
- **[Testing Stack Overflow](https://stackoverflow.com/questions/tagged/python+testing)** - Q&A for testing issues
- **[pytest Google Group](https://groups.google.com/g/pytest-dev)** - pytest community

### Blogs and Newsletters
- **[Python Testing Blog](https://realpython.com/testing/)** - Real Python testing articles
- **[pytest Blog](https://blog.pytest.org/)** - Official pytest blog
- **[Testing in Python Newsletter](https://www.pythonbytes.fm/)** - Python testing news

### Podcasts
- **[Test & Code](https://testandcode.com/)** - Python testing podcast
- **[Python Testing Podcast](https://www.pythonpodcast.com/)** - Testing-focused episodes
- **[Talk Python](https://talkpython.fm/)** - General Python podcast with testing episodes

---

## 🛠️ Testing Tools Ecosystem

### Test Runners
- **[nose2](https://docs.nose2.io/)** - Extends unittest with plugins
- **[green](https://github.com/CleanCut/green)** - Clean test runner with colors
- **[ward](https://ward.readthedocs.io/)** - Modern test runner for Python

### Test Utilities
- **[factory-boy](https://factoryboy.readthedocs.io/)** - Test data generation
- **[faker](https://faker.readthedocs.io/)** - Fake data generation
- **[model-bakery](https://model-bakery.readthedocs.io/)** - Django model factories

### Performance Testing
- **[locust](https://locust.io/)** - Load testing framework
- **[pytest-benchmark](https://github.com/ionelmc/pytest-benchmark)** - Benchmarking tests
- **[memory-profiler](https://github.com/pythonprofilers/memory_profiler)** - Memory usage testing

---

## 📱 Mobile and Learning Resources

### Interactive Learning
- **[Exercism Python](https://exercism.org/tracks/python)** - Python exercises with testing
- **[Codecademy Testing](https://www.codecademy.com/learn/learn-intermediate-python-3)** - Interactive testing lessons
- **[LeetCode Testing](https://leetcode.com/problemset/all/)** - Algorithm problems with test cases

### Mobile Apps
- **[Python Testing Apps](https://play.google.com/store/search?q=python%20testing)** - Mobile testing resources
- **[Codecademy Go](https://www.codecademy.com/mobile)** - Mobile coding with testing

### Cheat Sheets
- **[unittest Cheat Sheet](https://www.codecademy.com/learn/learn-intermediate-python-3/modules/testing-with-python/cheatsheet)** - unittest reference
- **[pytest Cheat Sheet](https://www.cheatography.com/davechild/cheat-sheets/pytest/)** - pytest quick reference
- **[Testing Patterns](https://martinfowler.com/bliki/TestDrivenDevelopment.html)** - Common testing patterns

---

## 🚀 Advanced Topics

### Testing Strategies
- **[Testing Pyramid](https://martinfowler.com/bliki/TestPyramid.html)** - Balancing test types
- **[Testing Trophy](https://kentcdodds.com/blog/write-tests)** - Modern testing approach
- **[Testing Quadrants](https://lisacrispin.com/2011/11/08/using-the-testing-quadrants/)** - Agile testing matrix

### Quality Metrics
- **[Code Coverage Metrics](https://www.atlassian.com/continuous-delivery/software-testing/code-coverage)** - Understanding coverage
- **[Mutation Testing](https://infection.github.io/)** - Testing test quality
- **[Test Maturity Model](https://www.thoughtworks.com/insights/blog/test-maturity-model)** - Testing evolution

### Specialized Testing
- **[Security Testing](https://owasp.org/www-project-top-ten/)** - Security-focused testing
- **[Accessibility Testing](https://www.w3.org/WAI/test-evaluate/)** - A11y testing
- **[Performance Testing](https://k6.io/)** - Load and stress testing

---

## 🎯 Testing in Production

### Monitoring and Observability
- **[Sentry](https://sentry.io/)** - Error tracking and monitoring
- **[DataDog](https://www.datadoghq.com/)** - Application monitoring
- **[New Relic](https://newrelic.com/)** - Performance monitoring

### A/B Testing
- **[Optimizely](https://www.optimizely.com/)** - A/B testing platform
- **[Google Optimize](https://optimize.google.com/)** - Free A/B testing
- **[VWO](https://vwo.com/)** - Conversion optimization

### Experimentation Platforms
- **[LaunchDarkly](https://launchdarkly.com/)** - Feature flag management
- **[Split.io](https://www.split.io/)** - Feature experimentation
- **[Statsig](https://statsig.com/)** - Product experimentation

---

**Last Updated**: February 2026
**Total Resources**: 80+ links
**Categories**: Documentation, Frameworks, Best Practices, Tools, Community