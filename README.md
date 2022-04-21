# Aptos-Node
code to run aptos node
curl -s https://raw.githubusercontent.com/AlekseyMoskalev1/script/main/aptos > aptos.sh && chmod +x aptos.sh && ./aptos.sh
UPDATE 1 
curl -s https://raw.githubusercontent.com/AlekseyMoskalev1/script/main/aptos_update > aptos_update.sh && chmod +x aptos_update.sh && ./aptos_update.sh
UPDATE 2 
cd aptos
docker compose down -v

docker pull aptoslab/validator:devnet
docker pull aptoslab/tools:devnet

rm *
rm $HOME/aptos/identity/id.json

wget -P $HOME/aptos https://raw.githubusercontent.com/aptos-labs/aptos-core/main/docker/compose/public_full_node/docker-compose.yaml
wget -P $HOME/aptos https://raw.githubusercontent.com/aptos-labs/aptos-core/main/docker/compose/public_full_node/public_full_node.yaml
wget -P $HOME/aptos https://devnet.aptoslabs.com/genesis.blob
wget -P $HOME/aptos https://devnet.aptoslabs.com/waypoint.txt
docker compose up -d
UPDATE 3
sudo wget -qO /usr/local/bin/yq https://github.com/mikefarah/yq/releases/latest/download/yq_linux_amd64  &> /dev/null
sudo chmod a+x /usr/local/bin/yq
cd $HOME/aptos
wget -O seeds.yaml https://raw.githubusercontent.com/razumv/helpers/main/aptos/seeds.yaml
/usr/local/bin/yq ea -i 'select(fileIndex==0).full_node_networks[0].seeds = select(fileIndex==1).seeds | select(fileIndex==0)' $HOME/aptos/public_full_node.yaml $HOME/aptos/seeds.yaml
rm $HOME/aptos/seeds.yaml
sudo docker compose restart
