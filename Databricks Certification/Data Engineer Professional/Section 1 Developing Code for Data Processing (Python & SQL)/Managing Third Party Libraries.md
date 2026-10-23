## Topic Overview
Managing third party libraries is a critical part of building real world data pipelines on Databricks. Your code often depends on external packages like pandas, numpy, requests, or domain specific libraries. The challenge is getting these libraries available on the compute where your code runs, and managing dependency conflicts across different jobs and notebooks.
Databricks gives you multiple ways to manage libraries depending on your use case. You can attach libraries directly to clusters, install them dynamically in notebooks, embed them in Databricks Asset Bundles (DABs) with requirements files, or store them in Unity Catalog volumes for central management. Each approach has tradeoffs in terms of reproducibility, ease of use, and scope (cluster wide vs notebook wide).
For the Professional exam, you need to understand when to use each method and how to properly specify dependencies in production code. The exam tests your knowledge of library attachment, notebook scoped libraries with %pip, requirements.txt in DABs, and how to avoid version conflicts.
---
## Key Concepts
Cluster Libraries vs Notebook Scoped Libraries
Cluster libraries are installed on the cluster itself and available to all users and notebooks on that cluster. This is the traditional approach and is useful when many jobs need the same dependency. Notebook scoped libraries using %pip are installed dynamically within a single notebook session and do not affect the cluster or other notebooks. Notebook scoped libraries are ideal for experiments, quick iterations, and when you want isolation between different notebooks.
Init Scripts for Custom Library Management
Init scripts are bash scripts that run when a cluster starts. You can use them to install packages, set environment variables, or run any startup logic. For library management, init scripts are useful when you need to install packages from a custom PyPI index, set up credentials for private repos, or install system level dependencies that packages depend on. However, init scripts add startup time and can be harder to debug than declarative approaches.
Requirements.txt in Databricks Asset Bundles
When deploying code via Databricks Asset Bundles, you define dependencies in a requirements.txt file (following Python standards) and reference it in your databricks.yml config. The DAB deployment process handles installing these libraries on the job cluster. This is the recommended approach for production pipelines because dependencies are version controlled, reproducible, and clearly documented alongside your code.
Unity Catalog Volumes for Library Storage
Unity Catalog volumes provide a centralized, governed way to store and distribute Python wheel files or JAR files across your workspace. Instead of uploading libraries to individual clusters, you store them in a UC volume and reference them from any cluster. This approach is useful in regulated environments where you need to track lineage of library assets and control who can access which libraries.
---
## Code Examples
### Notebook Scoped Installation with %pip
```python
%pip install requests==2.28.1 --quiet
# Restart Python to use the new library
%restart_python
import requests
response = requests.get('https://api.example.com/data')
print(response.json())
```
### Requirements.txt in a DAB
```yaml
# File: requirements.txt
pandas==1.5.3
numpy==1.24.1
scikit-learn==1.2.0
pyarrow>=10.0.0
# File: databricks.yml
resources:
  jobs:
    my_job:
      name: Data Processing Job
      tasks:
        - task_key: process
          python_wheel_task:
            package_name: my_package
            entry_point: run
          job_cluster_key: compute
      job_clusters:
        - job_cluster_key: compute
          new_cluster:
            spark_version: 13.3.x-scala2.12
            node_type_id: i3.xlarge
            num_workers: 2
            python_requirements:
              - /Workspace/requirements.txt
```
### Init Script for Custom Package Index
```bash
#!/bin/bash
# Save as init_script.sh and attach to cluster
# Install from private PyPI index
pip install --index-url https://private-pypi.example.com/simple mycompany-utils==1.0.0
# Install system dependencies
apt-get update && apt-get install -y libxml2-dev
# Set environment variable
export MY_API_KEY="${DATABRICKS_SECRET_API_KEY}"
```
---
## Common Exam Scenarios
A developer is creating a DAB with a Lakeflow job that requires the requests library. The requirements must be deterministic and version pinned. The job runs on a job cluster that is created fresh each time. What is the correct approach? The answer involves specifying dependencies in a requirements.txt file in the DAB repository and referencing it in the databricks.yml config. This ensures reproducibility because the exact versions are checked in with the code. Using %pip in a notebook would work for exploration but is not suitable for production jobs in a DAB because notebook scoped libraries only persist within that notebook session and would be lost on cluster restart.
A data engineer needs to ensure that a specific version of pandas is available across all notebooks running on a shared all purpose cluster. Multiple teams use this cluster and they want to avoid version conflicts. The best approach is to attach the library to the cluster itself (via the Clusters UI or API) with a specific version constraint. This way, all notebooks and jobs using that cluster get the same library version. The alternative of using %pip in each notebook would be tedious and error prone because each notebook owner would need to install it independently.
An organization has private Python packages hosted on an internal PyPI server. Developers need these packages installed on all clusters. What approach works best? Init scripts are the right choice here. The script can authenticate to the private server, install the packages, and any cluster using that init script will have the packages available at startup. This is better than manually installing with %pip because it is consistent and automated, and better than attaching cluster libraries because init scripts handle authentication and custom indices more naturally.
---
## Key Takeaways
- Use requirements.txt in DABs for production code to ensure dependencies are version controlled and reproducible.
- Use %pip in notebooks for interactive development and testing, not for production jobs.
- Attach libraries directly to clusters when multiple notebooks/jobs on the same cluster need the same dependency.
- Use init scripts for system level dependencies, private package indices, or complex setup logic.
- Use UC volumes to centrally store and govern wheel files and JARs, especially in regulated environments.
---
## Gotchas and Tips
⚠️ After installing a library with %pip, always run %restart_python to restart the Python interpreter. Otherwise, the new library might not be available in the current session.
⚠️ Init scripts run as the root user on the cluster. Avoid setting user specific paths or permissions that might not work across different users.
⚠️ If you attach conflicting library versions to a cluster (e.g., two versions of the same package), the cluster startup will fail. Always be careful when updating cluster libraries on shared clusters.
---
## Links and Resources
- **[Databricks Libraries](https://docs.databricks.com/aws/en/libraries/)** — Installing and managing third-party libraries on clusters and notebooks.
- **[Cluster Libraries](https://docs.databricks.com/aws/en/libraries/cluster-libraries)** — Managing cluster-scoped library installations and dependency resolution.