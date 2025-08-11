# RevBugBench
This document contains information on how to run the RevBugBench benchmarks

### Setup
1. Clone the repository
```sh
git clone https://github.com/ajoe2/fuzzbench.git
cd fuzzbench
```
2. Install the required Python dependencies by running
```sh
make install-dependencies
```
3. Create a file called `experiment-config.yaml` with the following contents:
```sh
# The number of trials of a fuzzer-benchmark pair.
trials: 10

# The amount of time in seconds that each trial is run for.
# 1 day = 24 * 60 * 60 = 86400
max_total_time: 86400

# The location of the docker registry.
# FIXME: Support custom docker registry.
# See https://github.com/google/fuzzbench/issues/777
docker_registry: gcr.io/fuzzbench

# The local experiment folder that will store most of the experiment data.
# Please use an absolute path.
experiment_filestore: /path/to/experiment-data

# The local report folder where HTML reports and summary data will be stored.
# Please use an absolute path.
report_filestore: /path/to/report-data

# Flag that indicates this is a local experiment.
local_experiment: true
```
Make sure to create directories for experiment_filestore/report_filestore and replace `/path/to/experiment-data` and `/path/to/report-data` with the correct locations. For example,
```sh
# The local experiment folder that will store most of the experiment data.
# Please use an absolute path.
experiment_filestore: /data2/ajoe/fuzzbench_data/experiment-data

# The local report folder where HTML reports and summary data will be stored.
# Please use an absolute path.
report_filestore: /data2/ajoe/fuzzbench_data/report-data
```

### Running the benchmark
1. Activate the virtualenv by running
```sh
source .venv/bin/activate
```
2. Run
```sh
PYTHONPATH=. python3 experiment/run_experiment.py \
--experiment-config experiment-config.yaml \
--benchmarks binutils-fuzz_cxxfilt binutils-fuzz_disassemble curl lcms libpcap libxml2_reader libxml2_xml proj4 usrsctp zstd \
--experiment-name $EXPERIMENT_NAME \
--fuzzers aflplusplus \
--runners-cpus $NUMBER_OF_CPUS
```
Make sure to replace `$EXPERIMENT_NAME` and `$NUMBER_OF_CPUS` with good values for the experiment. 
3. At this point, RevBugBench should be running. For full documentation details, see https://google.github.io/fuzzbench/running-a-local-experiment