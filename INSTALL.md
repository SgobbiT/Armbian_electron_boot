Procedura per l'avvio automatico di un'applicazione electron al posto del desktop manager

N.B. Attualmente X non parte...
Installando il desktop completo parte (TODO cercare i pacchetti mancanti)
```shell
apt-get install task-mate-desktop
```

# Installazione

## Prima parte
Procedura setup come utente root

```shell
# Installare Armbian Debian server (http://www.armbian.com/download/)

# Aggiungere il repository nodejs
echo "deb http://deb.nodesource.com/node_10.x stretch main" >> /etc/apt/sources.list
wget -qO- https://deb.nodesource.com/gpgkey/nodesource.gpg.key | apt-key add -

# Aggiornamento e installazione pacchetti necessari
apt-get update
apt-get upgrade -y
#apt-get install -y git vim curl wget zip # dev
#apt-get install -y make gcc g++ # build
apt-get install -y ntp ntpdate # time
apt-get install -y xinit # x11
apt-get install -y nodejs libnotify4 gconf2 libnss3 # electron

# Impostazione fuso orario
dpkg-reconfigure tzdata

# Installazione e setup di nodm display manager
apt-get install -y nodm
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

