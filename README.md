# OpenAir
<b>OpenAirInterface</b>

Download the repos separately (HSS, MME, SPGW-U and SPGW-C)

Clone the openair-hss, openair-mme, openair-spgwu-tiny and spgw-c repos (with specific commits):

HSS (https://github.com/OPENAIRINTERFACE/openair-hss/commit/d1d0e45ec749fc49f749c65744084ee09ab1f3e5)

    git clone https://github.com/OPENAIRINTERFACE/openair-hss.git
    git branch
    git checkout -b d1d0e45ec749fc49f749c65744084ee09ab1f3e5

MME (https://github.com/OPENAIRINTERFACE/openair-mme/tree/7b19f50222283e9b4f4a86d3adadc719871abca5)

    git clone https://github.com/OPENAIRINTERFACE/openair-mme.git
    git branch
    git check out -b 7b19f50222283e9b4f4a86d3adadc719871abca5

SPGW-U (https://github.com/OPENAIRINTERFACE/openair-spgwu-tiny/commit/c7927169b93456af59818d5682d66980bfee874d)
    
    git clone https://github.com/OPENAIRINTERFACE/openair-spgwu-tiny.git
    git branch
    git check out -b c7927169b93456af59818d5682d66980bfee874d

SPGW-C (https://github.com/OPENAIRINTERFACE/openair-spgwc/tree/9c2e49fe60351b69a7da1a898e4bbc0fd39dcc41)

    git clone https://github.com/OPENAIRINTERFACE/openair-spgwc.git
    git branch
    git checkout -b 9c2e49fe60351b69a7da1a898e4bbc0fd39dcc41

<b>Install Everything</b>

We can install first then go configure everything.

<b>Install Cassandra Database for the HSS</b>
There are issues with the current Cassandra script. First, add the public key:

    wget -q -O - https://www.apache.org/dist/cassandra/KEYS | sudo apt-key add -

Second, we are going to make 2 changes to a file. Open up the file “build/tools/build_helper.cassandra.” Change line 57 to

    $SUDO $INSTALLER install $OPTION curl openjdk-8-jre

Change line 64 to

    curl https://downloads.apache.org/cassandra/KEYS | $SUDO  apt-key add -

Then install:

    ./build_cassandra --check-installed-software --force

<b>Install HSS</b>

    ./build_hss_rel14 --check-installed-software --force
    ./build_hss_rel14 --clean

<b>Install MME</b>

    ./build_mme --check-installed-software --force
    ./build_mme --clean

<b>Install SPGW-U</b>

The SPGW-U is now in the openair-spgwu-tiny repository.

There is a dependency issue for the “folly” package requirement. “fmt” will be installed.

    cd ~/openair-spgwu-tiny/build/scripts
    ./build_spgwu -I -f
    ./build_spgwu -c -V -b Debug -j

<b>Install SPGW-C</b>

The SPGW-C is now in the openair-spgwu repository.

    cd ~/openair-spgwc/build/scripts
    ./build_spgwc -I -f
    ./build_spgwc -c -V -b Debug -j

<b>Configure Everything</b>

To make changes easier, we can change the permisions on the folder where all the config files will be.

    sudo mkdir /usr/local/etc/oai
    sudo chmod 777 -R oai/

<b>Configure Cassandra</b>

If the ealier installation was good, you should be able to run the following command.

    nodetool status

Stop Cassandra and cleanup the log files before modifying the configuration. This is blatently stolen from the official guide.

    sudo service cassandra stop
    sudo rm -rf /var/lib/cassandra/data/system/*
    sudo rm -rf /var/lib/cassandra/commitlog/*
    sudo rm -rf /var/lib/cassandra/data/system_traces/*
    sudo rm -rf /var/lib/cassandra/saved_caches/*

Update /etc/cassandra/cassandra.yaml. HERE is a version. The summary of the changes is below:

    Line 10: change cluster_name to ‘HSS Cluster’
    Line 273: `seeds: “127.0.0.1”
    Line 386: listen_address: localhost
    Line 444: rpc_address: localhost
    Line 705: endpoint_snitch: GossipingPropertyFileSnitch

Restart Cassandra:

    sudo service cassandra start

