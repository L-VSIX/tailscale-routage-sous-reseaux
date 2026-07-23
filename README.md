# 🔐 Configuration du routage des sous-réseaux — Tailscale

> Accès distant complet au homelab depuis n'importe quel terminal mobile, sans exposer aucun port public côté box opérateur.

## Déploiement

Tailscale est déployé sous forme de conteneur **LXC Debian**.

## Particularité LXC — accès au tunnel

Tailscale nécessite l'accès au périphérique `/dev/net/tun`, indisponible par défaut dans un conteneur non privilégié. Deux directives à ajouter côté hôte Proxmox, dans le fichier de configuration du conteneur :

```
lxc.cgroup2.devices.allow: c 10:200 rwm
lxc.mount.entry: /dev/net/tun dev/net/tun none bind,create=file
```

## Routage des sous-réseaux (subnet routes)

Le nœud LXC Tailscale annonce les sous-réseaux VLAN internes du homelab (advertise-routes), ce qui permet à un client Tailscale distant d'atteindre directement n'importe quel hôte du LAN interne — pas seulement le nœud Tailscale lui-même — une fois la route approuvée côté console d'administration Tailscale.

```bash
root@tailscale:~# tailscale up --advertise-routes=192.168.6.0/24,192.168.26.0/24,192.168.46.0/24,192.168.56.0/24,192.168.66.0/24 --reset
root@tailscale:~# tailscale set --advertise-exit-node
```

## Bénéfice de l'approche

Installation en quelques minutes sur Windows, macOS, Linux, iOS, Android ou des serveurs cloud.
Pas besoin de gérer un serveur VPN central ni de configurer des certificats.

## Repos liés

- [`opnsense-segmentation-vlan`](https://github.com/L-VSIX/opnsense-segmentation-vlan) — VLAN VPN dédié

## Auteur

**Lilian Vertueux** — [LinkedIn](https://www.linkedin.com/in/lilian-vertueux/)
