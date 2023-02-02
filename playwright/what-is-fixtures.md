
# What is Playwright Fixtures

刚开始接触 Playwright Fixtures 时，直接翻译 “夹具”，一直不明白它的概念，甚至感到深奥。
直到今天自己 wiki 搜索 fixture 名词的意思，再看了 pytest 关于 fixtures 的解释才有了一点点概念，在这里做个记录。

## Wiki Fixture

[Fixture (tool)](https://en.wikipedia.org/wiki/Fixture_(tool))
> A fixture is a work-holding or support device used in the manufacturing industry.[1][2] Fixtures are used to securely locate (position in a specific location or orientation) and support the work, ensuring that all parts produced using the fixture will maintain conformity and interchangeability. Using a fixture improves the economy of production by allowing smooth operation and quick transition from part to part, reducing the requirement for skilled labor by simplifying how workpieces are mounted, and increasing conformity across a production run.[2]

[Software Test fixture](https://en.wikipedia.org/wiki/Test_fixture)
> A software test fixture sets up a system for the software testing process by initializing it, thereby satisfying any preconditions the system may have.[1] For example, the Ruby on Rails web framework uses YAML to initialize a database with known parameters before running a test.[2] This allows for tests to be repeatable, which is one of the key features of an effective test framework.[1]

## Pytest Fixture
[Docs: About fixtures](https://docs.pytest.org/en/latest/explanation/fixtures.html#about-fixtures)
> In testing, a fixture provides a defined, reliable and consistent context for the tests. This could include environment (for example a database configured with known parameters) or content (such as a dataset).

在测试中，一个 fixture 为测试提供了一个确定的、可靠的和一致的环境。这可能包括环境（例如用已知参数配置的数据库）或内容（如数据集）。

> Fixtures define the steps and data that constitute the arrange phase of a test (see Anatomy of a test). In pytest, they are functions you define that serve this purpose. They can also be used to define a test’s act phase; this is a powerful technique for designing more complex tests.

fixtures 定义了构成测试运行阶段的步骤和数据（见测试的剖析）。在 pytest 中，它们是你定义的用于此目的的函数。它们也可以用来定义测试的行为阶段；这是设计更复杂测试的强大技术。

> The services, state, or other operating environments set up by fixtures are accessed by test functions through arguments. For each fixture used by a test function there is typically a parameter (named after the fixture) in the test function’s definition.

测试函数通过参数访问由 fixtures 设置的服务、状态或其他操作环境。对于测试函数使用的每个 fixture，在测试函数的定义中通常有一个参数（以 fixture 命名）。