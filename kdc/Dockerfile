FROM debian:latest

# Définir l'argument pour éviter les invites interactives
ARG DEBIAN_FRONTEND=noninteractive

# Mise à jour et installation des paquets nécessaires en une seule commande pour réduire les couches
RUN apt-get update && \
    apt-get install -y --no-install-recommends apt-utils krb5-kdc && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Création des répertoires nécessaires
RUN mkdir -p /var/log/kerberos /etc/kdc

# Déplacement et création de liens symboliques pour krb5.conf
RUN mv /etc/krb5.conf /etc/kdc/krb5.conf && \
    ln -s /etc/kdc/krb5.conf /etc/krb5.conf

# Initialisation de la base de données Kerberos
RUN kdb5_util create -s -P password -r EXAMPLE.ORG

# Exposer le port Kerberos
EXPOSE 88

# Commande par défaut pour démarrer le service
CMD ["/usr/sbin/krb5kdc", "-n"]
