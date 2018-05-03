# install

## Prima parte
Procedura setup come utente root

```shell
# install debian using [netinst](https://www.debian.org/CD/netinst/)

# Aggiornamento e installazione pacchetti necessari
aptitude update
aptitude upgrade -y
aptitude install -y git vim curl wget zip # dev
aptitude install -y make gcc g++ # build
aptitude install -y ntp ntpdate # time
aptitude install -y xinit # x11
aptitude install -y nodejs npm libnotify4 gconf2 libnss3 # electron

# Impostazione fuso orario
mv /etc/localtime /etc/localtime.old
ln -s /usr/share/zoneinfo/Europe/Rome /etc/localtime
pkill ntpd
ntpdate pool.ntp.org
systemctl start ntp.service

# Installazione e setup di nodm display manager
aptitude install -y nodm
sed -i "s/NODM_ENABLED=false/NODM_ENABLED=true/" /etc/default/nodm
sed -i "s/NODM_USER=root/NODM_USER=${user}/" /etc/default/nodm
sed -i "s/NODM_XSESSION=\/etc\/X11\/Xsession/NODM_XSESSION=\/home\/${user}\/.xinitrc/" /etc/default/nodm

# Installazione electron
npm -g --unsafe-perm install electron
```

## Seconda parte
Procedura setup come utente ${user},

```shell
# Setup di .xinitrc per l'avvio dell'app electron
echo "exec electron $HOME/boot" > $HOME/.xinitrc
chmod +x $HOME/.xinitrc

# Copiare la propria app electron nella sottocartella boot nella home
git clone git://github.com/ahdinosaur/bonetron $HOME/boot
```
