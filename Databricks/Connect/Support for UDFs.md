Databricks Connect for Python supports [user-defined functions (UDF)](https://docs.databricks.com/aws/en/udf/). When a DataFrame operation that includes UDFs is executed, the UDFs are serialized by Databricks Connect and sent to the server as part of the request.
# Manage UDF dependencies

> ==**Preview**==  
> As of July 2026. This feature is in [Public Preview](https://docs.databricks.com/aws/en/release-notes/release-types) and requires Databricks Connect for Python 16.4 or above, and a cluster running Databricks Runtime 16.4 or above. To use this feature, enable the preview **Enhanced Python UDFs in Unity Catalog** in your workspace.
Databricks Connect supports specifying Python dependencies that are required for UDFs. These dependencies are installed on Databricks compute as part of the UDF's Python environment.
This feature allows users to specify dependencies that the UDF needs in addition to the packages provided in the base environment. It can also be used to install a different version of the package from what is provided in the [base environment](https://docs.databricks.com/aws/en/dev-tools/databricks-connect/python/udf#base-env).
Dependencies can be installed from the following sources:
- PyPI packages
- Packages stored in Unity Catalog volumes
- Local packages, folders, and Python files
To include custom dependencies in your UDF, specify them in an environment using `withDependencies`, then use that environment to create a Spark session. The dependencies are installed on your Databricks compute and will be available in all UDFs that use this Spark session.
The following code declares the PyPI package `dice` as a dependency:
```python
from databricks.connect import DatabricksSession, DatabricksEnv
env = DatabricksEnv().withDependencies("dice==3.1.0")
spark = DatabricksSession.builder.withEnvironment(env).getOrCreate()
```
## **Behavior in Databricks notebooks and jobs**
In notebooks and jobs, UDF dependencies need to be installed directly in the REPL. Databricks Connect validates the REPL Python environment by verifying that all specified dependencies are already installed and throws an exception if any are not installed. Notebook environment validation runs for both PyPI and Unity Catalog volume dependencies, but not for local dependencies.
## Limitations
https://docs.databricks.com/aws/en/dev-tools/databricks-connect/python/udf#limitations
# Automatic management of UDF dependencies

> ==**Preview**==  
> This feature is in [Public Preview](https://docs.databricks.com/aws/en/release-notes/release-types) and requires Databricks Connect for Python 18.1 or above, Python 3.12 on your local machine, and a cluster running Databricks Runtime 18.1 or above. To use this feature, enable the preview **Enhanced Python UDFs in Unity Catalog** in your workspace.
The Databricks Connect `withAutoDependencies()` API enables automatic discovery and upload of local modules and public PyPI dependencies used in the import statements in your UDFs. It removes the need to manually specify dependencies.
The following code enables automatic dependency management:
```python
from databricks.connect import DatabricksSession, DatabricksEnv
env = DatabricksEnv().withAutoDependencies(upload_local=True, use_index=True)
spark = DatabricksSession.builder.withEnvironment(env).getOrCreate()
```
The `withAutoDependencies()` method accepts the following parameters:
- `upload_local`: When set to `True`, local modules imported in your UDFs are automatically discovered, packaged, and uploaded to UDF sandbox.
- `use_index`: When set to `True`, public PyPI dependencies used in your UDFs are automatically discovered and installed on Databricks compute. The discovery process uses the installed packages on your local machine to determine versions, ensuring consistency between your local environment and the remote execution environment.
## **Limitations**
- Dynamic imports (for example, `importlib.import_module("foo")`) are not supported.
- Namespace packages (for example, `azure.eventhub` and `google.cloud.aiplatform`) are not supported.
- Dependencies installed using direct-URL references are not supported. This includes those installed from local wheel files.
- Dependencies installed from private package indices are not supported. Packages installed this way can't be distinguished from packages installed from the public PyPI.
- Dependency discovery doesn't work in a Python shell. Only Python scripts, IPython shell and Jupyter Notebooks are supported.
- Check more - https://docs.databricks.com/aws/en/dev-tools/databricks-connect/python/udf#limitations-1
## **Logging**
To output discovered dependencies, set the `SPARK_CONNECT_LOG_LEVEL` environment variable to `info` or `debug`. Alternatively, configure the Python logging framework:
```python
import logginglogging.basicConfig(level=logging.INFO)
```
The relevant logs are emitted by the `databricks.connect.auto_dependencies` module, for example:
```
DEBUG:databricks.connect.auto_dependencies.discovery:Discovered local module: my_json
DEBUG:databricks.connect.auto_dependencies.discovery:Discovered local module: my_helper
DEBUG:databricks.connect.auto_dependencies.discovery:Discovered distribution: simplejson for module simplejson
DEBUG:databricks.connect.auto_dependencies.discovery:Discovered distribution: dice for module dice
INFO:databricks.connect.auto_dependencies.hook:Synced zip artifact for: my_json
INFO:databricks.connect.auto_dependencies.hook:Synced zip artifact for: my_helper
INFO:databricks.connect.auto_dependencies.hook:Updated simplejson with auto-detected version ==3.20.2
INFO:databricks.connect.auto_dependencies.hook:Updated dice with auto-detected version ==4.0.0
```
# Python base environment
UDFs are executed on the Databricks compute and not on the client. The base Python environment in which UDFs are executed depends on the Databricks compute.
For clusters, the base Python environment is the Python environment of the Databricks Runtime version running on the cluster. The Python version and the list of packages in this base environment are found under the _System environment_ and _Installed Python libraries_ sections of the [Databricks Runtime release notes](https://docs.databricks.com/aws/en/release-notes/runtime/).
For serverless compute, the base Python environment corresponds to the [serverless environment version](https://docs.databricks.com/aws/en/release-notes/serverless/#environment-version) according to the following table. Databricks Connect versions not listed in this table either do not support serverless yet or have reached end-of-support. See [version support matrix](https://docs.databricks.com/aws/en/dev-tools/databricks-connect/requirements#versions) and [end-of-support Databricks Connect versions](https://docs.databricks.com/aws/en/dev-tools/databricks-connect/requirements#end-of-support-versions).
Check the table here - https://docs.databricks.com/aws/en/dev-tools/databricks-connect/python/udf#python-base-environment.
# References
- https://docs.databricks.com/aws/en/dev-tools/databricks-connect/python/udf