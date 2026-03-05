# 232R Group Project - Provident Vehicle Detection at Night (PVDN)

**Dataset:** [Provident Vehicle Detection at Night (PVDN)](https://www.kaggle.com/datasets/saralajew/provident-vehicle-detection-at-night-pvdn/data)

**Goal:** Data structure exploration, counts, missing values, duplicates, label distributions, and basic image metadata analysis using Apache Spark on SDSC Expanse.

---

## Running on SDSC Expanse

### 1. First-Time Setup (run once after login)

```bash
ln -sf /expanse/lustre/projects/uci157/$USER
ln -sf /expanse/lustre/projects/uci157/esolares
```

### 2. Get the Repo

**First time:**
```bash
git clone https://github.com/jestr-collab/232R-group-project.git
cd 232R-group-project
```

**Already cloned:**
```bash
cd 232R-group-project
git pull origin main
```

### 3. Submit the Job

The SLURM script `run_pvdn_eda.sh` is pre-configured with the correct account, partition, cores, and memory. Simply run:

```bash
sbatch run_pvdn_eda.sh
```

| SLURM Setting   | Value            |
|-----------------|------------------|
| Account         | `TG-SEE260003`   |
| Partition       | `debug`          |
| Cores           | 8                |
| Memory          | 128 GB           |
| Wall Time       | 30 min           |
| Output          | `logs/pvdn_<jobid>.out` |

