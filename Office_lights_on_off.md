# Setup so that office lights are turned on/off with PC
Using Philips Hue REST API and simple scripting in a oneshot service.

## Requirements
* Work!

### Tasks
- [X] Create Dev account with Hue, to get access to V2 docs
- [X] Test with simple curl calls if possible
- [X] Create script to send on/off command to Hue
- [X] Create Oneshot service to trigger when turning PC on/off
- [ ] Test


### Creating an account
Quit simple, just head to their [website](https://developers.meethue.com/develop/hue-api-v2/) and sign up.
After sign up you need to verify your email, after that your good to go. 


### Making a simple test call
I was lazy and used the V1 API to get started.
Hue have a good getting startet guide for this on their [site](https://developers.meethue.com/develop/get-started-2/).

I simply followed the guide to to "create" a user account on my Hue hub.

Then i ran a test with this command.
```bash
curl -H PUT https://10.0.0.57/api/1028d66426293e821ecfd9ef1a0731df/lights/1/state -d'{"on":false}'
```

And that light turned off 🥳
So POC done.


### Creating a script send the commands
I then started making a script which would take 1 parameter with 2 options 'on' and 'off'.

After a bit of tinkering and testing it ended up with this little script.

```bash
#!/bin/ksh

# Argument check
if [ $# -eq 0 ] ; then
  echo "No arguments supplied"
  exit 1
fi

if [ -z "$1" ] ; then
  echo "No argument supplied"
  exit 2
fi
ACTION=$1

# Argument value check
if [[ "$ACTION" == "on" ]] ; then
  COMMAND="true"
elif [[ "$ACTION" == "off" ]] ; then
  COMMAND="false"
else
  exit 3
fi

# Values
HOST="10.0.0.57"
KEY="xxxxxxxxxxxx"
ROOM_NAME="Kontor"
WRKFILE=/tmp/hue_script

# Init variables
ROOM_ID=0
GROUP_ID=0
GROUP_OWNER_ID=0
ROOM_NAME_WRK


ping -c 1 $HOST >/dev/null 2>&1
OK=$?

if [ $OK -ne 0 ] ; then
  echo "Ping to $HOST failed!"
  exit 4
fi
echo -e "Ping test to '$HOST', OK\n"

do_curl() {
  TYPE=$1
  RESOURCE=$2
  DATA=""

  if [ ! -z "$3" ] ; then
    DATA="-d $3"
  fi

  curl -k --header "hue-application-key: $KEY" --silent           \
       -X $TYPE "https://$HOST/clip/v2/resource/$RESOURCE" $DATA   \
       --write-out %{http_code} --output $WRKFILE.reply | read STATUS

  if [ $STATUS -ne 200 ] ; then
    echo "ERROR: HTTP code: $STATUS" >&2
    exit 5
  fi
  cat $WRKFILE.reply
}

#### GET ROOM ####
echo -e "Quering rooms\n"
do_curl GET room > $WRKFILE.wrk
jq -c '.data[]' $WRKFILE.wrk > $WRKFILE.rooms

echo -e "Recived data, parsing data\n"
while read i; do
  ROOM_NAME_WRK=$(echo "$i" | jq '.metadata.name' | sed 's/"//g')
  
  if [[ "$ROOM_NAME_WRK" == "$ROOM_NAME" ]] ; then
    ROOM_ID=$(echo "$i" | jq '.id' | sed 's/"//g')
    break
  fi
done < $WRKFILE.rooms

[ $ROOM_ID = 0 ] && exit 6

#### Get Light groups ####
echo -e "Found room '$ROOM_NAME', quering lightgroups\n"
do_curl GET grouped_light > $WRKFILE.wrk
jq -c '.data[]' $WRKFILE.wrk > $WRKFILE.groups

echo -e "Recived data, parsing data\n"
while read i; do
  GROUP_OWNER_ID=$(echo "$i" | jq '.owner.rid' | sed 's/"//g')
  
  if [[ "$GROUP_OWNER_ID" == "$ROOM_ID" ]] ; then
    GROUP_ID=$(echo "$i" | jq '.id' | sed 's/"//g')
    break
  fi
done < $WRKFILE.groups

[ $GROUP_ID = 0 ] && exit 7

#### Send command to group ####
echo -e "Found lightgroup for room, sending command\n"
do_curl PUT "grouped_light/$GROUP_ID" "{\"on\":{\"on\":$COMMAND}}" > /dev/null 2>&1

exit 0
```


### Creating a service
Before i created the service i created the folder /opt/zicy/scripts/ using `sudo mkdir -p /opt/zicy/scripts/` and made my user account owner of that folder using `sudo chown zicy:zicy /opt/zicy -R`.

I the  copied the script to that folder as 'hue.cmd'
I then made the script executable using `chmod +x hue.cmd`

After this i created the file 'zicy-hue.service' in the folder '/etc/systemd/system/' using the command `sudo vi /etc/systemd/system/zicy-hue.service`

I then added this content to the file
```bash
[Unit]
Description=Change hue light on/off during boot and shutdown
After=network.target

[Service]
Type=oneshot
ExecStart=/opt/zicy/scripts/hue.cmd on
RemainAfterExit=true
ExecStop=/opt/zicy/scripts/hue.cmd off
StandardOutput=journal

[Install]
WantedBy=multi-user.target
```

I then ran the command `sudo systemctl daemon-reload` because of the newly created service file.
After that i tested by doing `sudo systemctl start zicy-hue` and the light turned on, then i did `sudo systemctl stop zicy-hue` and the light turned off.

I then ran the command `sudo systemctl enable zicy-hue` so that the service will autostart on next boot.


### Testing
Well that comes now ...

---