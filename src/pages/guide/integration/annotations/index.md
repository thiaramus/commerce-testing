---
title: DocBlock Annotations | Commerce Testing
description: Declare context in your Adobe Commerce and Magento Open Source code with DocBlock annotations.
---

# DocBlock annotations

DocBlock annotations help to declare context in your code. In addition to built-in [PHPUnit annotations][]
, the Integration testing framework provides custom annotations described in this topic.

<InlineAlert variant="info" slots="text" />

Native PHP8 Attributes can be used instead or together with DocBlock annotations. This new method offers more flexibility and improves readability of the code. See the [Attributes](../attributes/index.md) to learn more. This feature is currently only available for Magento Open Source contributors. It will be released for general use with Magento Open Source 2.4.5.

### Quick overview

The following annotations are available in integration tests:

Name|Annotation|Format|Description
---|---|---|---
Admin Configuration Fixture|`@magentoAdminConfigFixture`|`@magentoAdminConfigFixture <config_path> <config_value>`| Configures an admin setting for the test run. For example, to enable Captcha, you would use `@magentoAdminConfigFixture admin/captcha/enable 1`.
Application Isolation|`@magentoAppIsolation`|`@magentoAppIsolation enabled/disabled`|Enables or disables application isolation when you run tests. When enabled, an application state after a test run will be the same as before the test run. For example, you should enable it, when you want to create sessions in a test, but you don't want them to affect other tests.
Configuration Fixture|`@magentoConfigFixture`|`@magentoConfigFixture [<store_code>_store] <config_path> <config_value>`|Sets up configuration settings for a particular test. The list of settings is stored in the `core_config_data` database table. After the test execution, the settings revert to their original state.
Database Isolation|`@magentoDbIsolation`|`@magentoDbIsolation enabled/disabled`|Enables or disables database isolation. Disabled by default, unless you are using `@magentoDataFixture`, in which case it is enabled by default. All data, required for a test, live during transaction only. Any test results won't be written in a database.
Data Fixture|`@magentoDataFixture`|`@magentoDataFixture <script_filename> / <method_name>/<fully_qualified_class_name> [as:alias with:{}]`|Points to a class or a method which creates testing entities (fixtures) for test execution. These are applied during the transaction.
Data Fixture Before Transaction|`@magentoDataFixtureBeforeTransaction`|`@magentoDataFixtureBeforeTransaction <script_filename>/<method_name>`|Points to a class or a method which creates testing entities (fixtures) for test execution before the transaction has begun. You will need to implement a rollback file for changes made here. (e.g. Fixture file my_fixture.php would also require a my_fixture_rollback.php that reverts the original fixture's changed.)
Application Area|`@magentoAppArea`|`@magentoAppArea adminhtml frontend global`|Configures test environment in the context of specified application area.
Enable/Disable Cache|`@magentoCache`|`@magentoCache all enabled disabled`|Enables or disables certain cache segment or all of them to prevent isolation problems.
Indexer Dimension Mode|`@magentoIndexerDimensionMode`|`@magentoIndexerDimensionMode <indexer> <mode>`|Sets the indexer dimension mode for the test run. More information can be found in the [DevBlog](https://community.magento.com/t5/Magento-DevBlog/Indexers-parallelization-and-optimization/ba-p/104922).
Register Components|`@magentoComponentsDir`|`@magentoComponentsDir <dir_path>`|Registers fixture components from specified directory (recursively). Unregisters the components after the test is finished.

### Applying annotations

The Magento-specific annotations for integration tests are applied in the following order:

1. `@magentoAppIsolation`
1. `@magentoDbIsolation`
1. `@magentoDataFixtureBeforeTransaction`
1. `@magentoDataFixture`
1. `@magentoIndexerDimensionMode`
1. `@magentoComponentsDir`
1. `@magentoAppArea`
1. `@magentoCache`
1. `@magentoAdminConfigFixture`
1. `@magentoConfigFixture`

This order is necessary to meet the requirement of setting up the store-scoped configuration values for fixture stores (stores that are created by data fixtures).

<!-- LINK DEFINITIONS -->

[PHPUnit annotations]: https://phpunit.readthedocs.io/en/9.5/annotations.html
