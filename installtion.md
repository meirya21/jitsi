HI

after installing the helm chart with the command:
`helm install jitsi -n jitsi -f values.yaml jitsi/jitsi-meet`
`kubctl apply -n jitsi -f ingress.yml`

i've done some modification in the pods (i tried to do this via the chart template but the configuration was not implemented)

in the JVB pod i enterd /etc/jitsi//videobridge/
there i made the following changes in the sip-communicator.properties:

org.ice4j.ice.harvest.DISABLE_AWS_HARVESTER=true
org.ice4j.ice.harvest.STUN_MAPPING_HARVESTER_ADDRESSES=meet-jit-si-turnrelay.jitsi.net:443
org.jitsi.videobridge.ENABLE_STATISTICS=true
org.jitsi.videobridge.STATISTICS_TRANSPORT=muc
org.jitsi.videobridge.xmpp.user.shard.HOSTNAME: jitsi.dev.aks.linnovate.net
org.jitsi.videobridge.xmpp.user.shard.DOMAIN: jitsi.dev.aks.linnovate.net  
org.ice4j.ice.harvest.NAT_HARVESTER_LOCAL_ADDRESS: 10.0.89.213   #this is the cornet internal ip address of the JVB service.             
org.ice4j.ice.harvest.NAT_HARVESTER_PUBLIC_ADDRESS: 51.105.253.103
org.jitsi.videobridge.xmpp.user.shard.USERNAME=jvb
org.jitsi.videobridge.xmpp.user.shard.PASSWORD=brE0XjOr
org.jitsi.videobridge.xmpp.user.shard.MUC_JIDS=JvbBrewery@internal.auth.localhost
org.jitsi.videobridge.xmpp.user.shard.MUC_NICKNAME=22c21298-442b-4dce-ac9e-b18c656c516c

then i restarted the service:
`service jitsi-videobridge2 restart`

also, I manually update the service configuration and added 4443 port (again didn't work to change it from the values.yml nor from the service.yml in the templats)


in the jitsi-prosody-0 pod i've tried to start the process with:

`service prosody start`

but got an error because the service cant find the configuration files in etc/prosody
the folder wasnt created so i created one and copy the whole /config/ folder to etc/prosody

after ive restarted the service:

`service prosody restart`

but got the errors:
"Failed to open server port 5222 on *, check that Prosody or another XMPP server is not already running and using this port"
"Failed to open server port 5280 on *, check that Prosody or another XMPP server is not already running and using this port"

without these changes the jitsi working for 2 clients.
with these configuration the service not able to connect (probaly because of the prosody service)

here are the web sites that i've worked with:

https://jitsi.github.io/handbook/docs/devops-guide/devops-guide-manual/
https://community.jitsi.org/t/after-upgrade-when-second-participant-enter-both-keeped-out/126930/4

for future use i need to scale it for a lot of clients 
so if you can please add a scaling option

