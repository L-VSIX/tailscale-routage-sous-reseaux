# 🔐 Configuration du routage des sous-réseaux — Tailscale

> Accès distant complet au homelab depuis n'importe quel terminal mobile, sans exposer aucun port public côté box opérateur.

## Déploiement

Tailscale est déployé sous forme de conteneur **LXC Debian**, complété par un **nœud secondaire sur Raspberry Pi** servant de point d'entrée à faible consommation, disponible même si les hyperviseurs principaux sont éteints.

## Particularité LXC — accès au tunnel

Tailscale nécessite l'accès au périphérique `/dev/net/tun`, indisponible par défaut dans un conteneur non privilégié. Deux directives à ajouter côté hôte Proxmox, dans le fichier de configuration du conteneur :

```
lxc.cgroup2.devices.allow: c 10:200 rwm
lxc.mount.entry: /dev/net/tun dev/net/tun none bind,create=file
```

## Routage des sous-réseaux (subnet routes)

Le nœud LXC Tailscale annonce les sous-réseaux VLAN internes du homelab (advertise-routes), ce qui permet à un client Tailscale distant d'atteindre directement n'importe quel hôte du LAN interne — pas seulement le nœud Tailscale lui-même — une fois la route approuvée côté console d'administration Tailscale.

## Bénéfice de l'approche

Aucun port n'est ouvert côté Livebox : tout le trafic d'administration à distance transite par le réseau maillé (mesh) Tailscale, avec authentification par identité plutôt que par exposition de service.

## Repos liés

- `opnsense-segmentation-vlan` — VLAN VPN dédié
- `raspberrypi-point-acces-wifi` — nœud secondaire partagé

## Auteur

**Lilian Vertueux** — [LinkedIn](https://www.linkedin.com/in/lilian-vertueux/)
