# A Report on Graph Indexing for Personalized PageRank Computation

## Prerequisites

- C++ 11
- GCC (GCC 4.8 is ok, we used 4.10)
- Boost (Boost 1.65 is ok, we used 1.74.0.3)
- cmake

We conducted the experiments on Debian 11. So, if you are using Debian or Ubuntu, try this command to install all the prerequisites:

```bash
sudo apt install git build-essential cmake libboost-all-dev
```

## Compile FORA code

```bash
git clone https://github.com/HKU-CS-9102/COMP9102.git
cd COMP9102/fora
cmake .
make
cd ..
```

## Download and extract the LiveJournal dataset

```bash
wget http://konect.cc/files/download.tsv.soc-LiveJournal1.tar.bz2
tar -xvjf download.tsv.soc-LiveJournal1.tar.bz2
```

## Convert the data file to approprite format

```bash
mkdir ./fora/data/livejournal
tail -n +3 ./soc-LiveJournal1/out.soc-LiveJournal1 > ./fora/data/livejournal/graph.txt  # remove the first 2 lines
echo $'n=4846610\nm=68475391' > ./fora/data/livejournal/attributes.txt
cd ./fora
```

## Build index for the livejournal dataset

```bash
nohup ./fora build --prefix ./data/ --dataset livejournal --epsilon 0.5 > logs/livejournal_build_index_kdd.out  2>&1 &
```

## Generate queries

```bash
nohup ./fora generate-ss-query --algo fora --prefix ./data/ --dataset livejournal --query_size 1000 > logs/livejournal_process_gen_query.log 2>&1 &
```

## Evaluate FORA with/without index

```bash
nohup ./fora query --algo fora --prefix ./data/ --dataset livejournal --epsilon 0.5 --query_size 20 > logs/livejournal_process_query_without_index_kdd.log 2>&1 &
```

```bash
nohup ./fora query --algo fora --prefix ./data/ --dataset livejournal --epsilon 0.5 --query_size 20 --with_idx > logs/livejournal_process_query_with_index_kdd.log 2>&1 &
```

You can find the example logs [here](fora/logs).
