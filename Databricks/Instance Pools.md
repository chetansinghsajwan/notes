**Instance Pools** are a managed cache of pre-configured, idle virtual machine (VM) instances. They are designed to dramatically reduce cluster startup and auto-scaling times.
Before pools were introduced, every time you launched a cluster or auto-scaled, Databricks had to request new VMs from your cloud provider (AWS, Azure, or GCP), wait for them to provision, boot up, and initialize. This process usually took several minutes. With pools, clusters can start and scale up **up to 4 times faster**.
### How Databricks Pools Work
Think of a pool as a **"waiting area"** or a parking lot for cloud resources.
1. **Pre-allocation:** Databricks provisions a set of idle VMs from your cloud provider based on configurations you define (e.g., instance type, RAM, storage).
2. **Cluster Creation:** When you or an automated job starts a cluster backed by that pool, Databricks immediately grabs an idle instance from the pool instead of waiting on the cloud provider.
3. **Dynamic Backfilling:** If the pool runs out of idle instances, it requests new ones from the cloud provider to expand. While a cluster is running, the pool will automatically "backfill" instances to maintain its minimum idle threshold.
4. **Recycling:** When a cluster completes its work or scales down, those instances are returned to the pool, ready to be immediately claimed by another cluster.
### Key Configuration Settings
When setting up a Databricks Pool, you control its size, behavior, and costs using three main parameters:
- **Minimum Idle Instances:** The minimum number of warm VMs the pool will _always_ keep ready. These instances do not terminate, ensuring instant availability even if no workloads are currently running.
- **Maximum Capacity:** The maximum total number of instances (idle + actively used by clusters) that the pool can provision. This acts as a hard ceiling to control cloud budgets or stay within cloud provider quota limits.
- **Idle Instance Auto Termination:** The number of minutes that "extra" instances (beyond your minimum idle count) are allowed to sit idle before being spun down to save costs.
### Advanced Features
- **Preloaded Runtimes:** You can configure the pool to pre-download a specific **Databricks Runtime (DBR)** version or a custom **Docker image** onto the idle instances. This shaves off even more startup time because the VM doesn't need to download software packages upon cluster initialization.
- **Autoscaling Local Storage:** Databricks pools automatically monitor disk space. If an instance's local storage runs low during heavy processing, it will dynamically attach additional managed disks (up to 5 TB per VM) without crashing your workload.
- **Spot Instances:** You can configure pools to utilize cloud Spot Instances instead of On-Demand instances, which heavily cuts infrastructure costs for non-production workloads.
### Cost Implications: What Do You Pay For?
- **Databricks DBUs:** Databricks **does not charge DBUs** (Databricks Units) while instances are sitting idle in the pool. You only pay DBUs when a cluster actively acquires a node to run code.
- **Cloud Infrastructure Provider:** Your cloud provider (AWS, Azure, GCP) **will charge you** for the underlying virtual machines and attached storage for as long as they are kept alive in the pool, even if they are sitting idle.

> 💡 **Best Practice:** To balance performance and cloud costs, keep your `Minimum Idle Instances` low (or at 0 for off-hours) and rely on a aggressive `Idle Instance Auto Termination` time (e.g., 10–15 minutes) for sporadic workloads.
Are you looking to optimize a specific workload, such as speeding up short-lived data engineering jobs or scaling out interactive analyst clusters?
# References
- [Connect to pools | Databricks on AWS](https://docs.databricks.com/aws/en/compute/pool-index)