# 232R Group Project - Provident Vehicle Detection at Night (PVDN)

**Dataset:** [Provident Vehicle Detection at Night (PVDN)](https://www.kaggle.com/datasets/saralajew/provident-vehicle-detection-at-night-pvdn/data)

**Goal:** Data structure exploration, counts, missing values, duplicates, label distributions, and basic image metadata analysis using Apache Spark on SDSC Expanse.

---

## Running on SDSC Expanse

### First-Time Setup (run once after login)

```bash
ln -sf /expanse/lustre/projects/uci157/$USER
ln -sf /expanse/lustre/projects/uci157/esolares
```

### Submit the Job

```bash
sbatch run_pvdn_eda.sh
```

### Monitor the Job

```bash
squeue -u $USER
tail -f logs/pvdn_<job_id>.out
seff <job_id>
```

### Cancel if Needed

```bash
scancel <job_id>
```

---

## SDSC Expanse Environment Setup

### Cluster Resources

We ran our Spark job on the [SDSC Expanse](https://www.sdsc.edu/services/hpc/expanse/) supercomputer using a single compute node with the following allocation:

| Resource       | Value   |
|----------------|---------|
| Total Cores    | 8       |
| Total Memory   | 128 GB  |
| Partition      | shared  |

### SparkSession Configuration (Expanse)

```python
spark = SparkSession.builder \
    .config("spark.driver.memory", "2g") \
    .config("spark.executor.memory", "18g") \
    .config("spark.executor.instances", 7) \
    .getOrCreate()
```

### Configuration Justification

Memory and executor settings were derived using the recommended formulas:

**Executor Instances:**

```
Executor Instances = Total Cores - 1
                   = 8 - 1
                   = 7
```

One core is reserved for the driver and cluster management overhead, leaving 7 cores for executor work.

**Executor Memory:**

```
Executor Memory = (Total Memory - Driver Memory) / Executor Instances
                = (128 GB - 2 GB) / 7
                = 126 GB / 7
                = 18 GB
```

The driver is kept at 2 GB since it only coordinates the job and does not process data directly. The remaining 126 GB is split evenly across 7 executors, giving each 18 GB — enough headroom for the JSON annotation parsing and image metadata operations in our EDA pipeline.

### Data Location on Expanse

```
DATA_ROOT = /expanse/lustre/projects/uci157/kkravchenko
```

The PVDN dataset was stored on the Lustre parallel filesystem for high-throughput I/O during Spark reads.

### Spark UI - Multiple Executors Active During Data Loading

![Spark UI Executors](spark_ui_executors.png)

*Screenshot showing 7 executors actively processing data during the initial JSON annotation load stage.*
