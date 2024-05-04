# Gevulot-Devnet-Local-Node
 > [Discord](https://discord.gg/3eYXCP2N)

CPU: 64+ cores
RAM: 512GB
Disk: 1 TB NVMe
Ubuntu 22.04

Sistem Gereksinimlerini yükleyelim
 
    sudo apt update
    sudo apt install -y software-properties-common uidmap
    sudo add-apt-repository -y ppa:projectatomic/ppa
    sudo apt update
    sudo apt install -y podman
    sudo apt install libssl-dev
    sudo apt install pkg-config
    sudo apt-get install protobuf-compiler
 
Dosyaları çekelim
  
    git clone https://github.com/gevulotnetwork/gevulot.git
    cd gevulot
    mkdir -p data/{db,node}
    podman run -d --rm --name gevulot-postgres --network host -v ./crates/node/migrations:/docker-entrypoint-initdb.d:z -v ./data/db:/var/lib/postgres/data:z --env POSTGRES_USER=gevulot --env POSTGRES_PASSWORD=gevulot --env POSTGRES_DB=gevulot docker.io/library/postgres:16-alpine
    cargo build --release

  Aşağıda çıkacak olan pubkeyi kaydedin
       
    ./target/release/gevulot generate key --key-file data/node/node.key
    sudo modprobe vhost_vsock
    screen -S gevulot
    ./target/release/gevulot run --data-directory ./data/node --log-directory ./data/node/log --node-key-file ./data/node/node.key
       CTRL A+D ile çıkalım

  Gevulot Node şuan screen içerinde çalışır durumdaysa devammmm

    curl https://ops.city/get.sh -sSfL | sh
    ./target/release/gevulot-cli generate-key
    ./target/release/gevulot peer <yukarıdan alınan_pubkey> whitelist
    ./target/release/gevulot show public-key --key-file data/node/node.key
    ./target/release/gevulot peer <yukarıdan alınan_pubkey> whitelist
    cd crates/tests/e2e-tests/
    cp ../../../target/release/{prover,verifier} .
    ops image create -n -c manifest/prover.json
    ops image create -n -c manifest/verifier.json
    Screen -S ops
    cargo run -- -p ~/.ops/images/prover -v ~/.ops/images/verifier -k ../../../localkey.pki
CTRL A+D ile çıkalım  
Çıktı aşağıdaki gibi ise problem yok 

...
Listening on <your ip address>
request file: 51a68fd499fa25fc5aa5abb5c9a1cb2e2060831c8c6df96c1196a277253865e6
request file: c47255b3c219c4fd7bdb72e2080d5d2b46ac297e4c52bb0ec26b6ffe0e376e90
    
    
