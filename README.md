#OpenVPN Running as a Docker Container 
These are the steps used to create a basic OpenVPN Docker Container using the docker image provided by kylemanna/openvpn. 

Droplet Login: 

Droplet Name: ubuntu-vpn-server-512mb-nyc1-01
IP Address: 67.205.186.47
Username: root
Password: de0af744b5945123

##Create Server on Digital Ocean 
1. Create a digital ocean account and create a small droplet. 

##Install Docker:
1. Follow instructions from here: https://docs.docker.com/engine/installation/linux/ubuntu/
2. Add docker user to the group if not already added. 

##Create a PKI Certificate Store: 
1. docker create -v /home/ovpn-data:/etc/openvpn --name ovpn-data ubuntu:16.04
2. docker run --volumes-from ovpn-data --rm kylemanna/openvpn ovpn_genconfig -u udp://67.205.186.47:1194
3. docker run --volumes-from ovpn-data --rm -it kylemanna/openvpn ovpn_initpki

##Launch the OpenVPN Server: 
1. docker run --volumes-from ovpn-data --rm -p 1194:1194/udp --cap-add=NET_ADMIN kylemanna/openvpn

##Generate Client Certificates:
1. docker run --volumes-from ovpn-data --rm -it kylemanna/openvpn easyrsa build-client-full client1 nopass
2. docker run --volumes-from ovpn-data --rm -it kylemanna/openvpn easyrsa build-client-full CLIENTNAME nopass
3. docker run --volumes-from ovpn-data --rm kylemanna/openvpn ovpn_getclient CLIENTNAME > CLIENTNAME.ovpn

##Test
1. Attach the provided client.ovpn file to a vpn client and connect. Verify the connection using ifconfig or as simply as checking ip-chicken.com. 
The IP Should be 67.205.186.47. 

References: https://github.com/kylemanna/docker-openvpn
