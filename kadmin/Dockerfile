FROM debian:latest

# Définir l'argument pour éviter les invites interactives
ARG DEBIAN_FRONTEND=noninteractive

# Mise à jour et installation des paquets nécessaires en une seule commande pour réduire les couches
RUN apt-get update && \
    apt-get install -y --no-install-recommends apt-utils krb5-admin-server && \
    echo "krb5-admin-server krb5-admin-server/newrealm note EXAMPLE.ORG" | debconf-set-selections && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Création des répertoires nécessaires
RUN mkdir -p /var/log/kerberos /etc/kadmin

# Copier le fichier de configuration ACL
COPY kadm5.acl /etc/krb5kdc/kadm5.acl

# Déplacement et création de liens symboliques pour krb5.conf
RUN mv /etc/krb5.conf /etc/kadmin/krb5.conf && \
    ln -s /etc/kadmin/krb5.conf /etc/krb5.conf

# Initialisation de la base de données Kerberos
RUN kdb5_util -P password -r EXAMPLE.ORG create -s

# Commande par défaut pour démarrer le service
CMD ["/usr/sbin/kadmind", "-nofork"]
