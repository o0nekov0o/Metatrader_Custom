# MetaTrader5 Docker Setup

## Prérequis
- Docker installé
- Compte Fusion Markets actif

## Démarrage
```bash
# Récupérer l'image
docker pull o0nekov0o/metatrader5-custom:latest

# Lancer le container
docker compose -f docker-compose.mt5.yml up -d
```

## Vérifier le port MT5
```bash
docker port metatrader5-custom
```
Le port 3000 interne est mappé dynamiquement.
Accéder à l'interface web : http://ton-ip:PORT_RETOURNÉ

## Pour fixer le port définitivement
Dans docker-compose.mt5.yml :
```yaml
ports:
  - "3000:3000"
  - "8001:8001"
```

## Démarrer le serveur RPyC
```bash
# Donner les permissions
docker exec metatrader5-custom chmod 777 /tmp/mt5linux

# Lancer le serveur
docker exec -d -u abc metatrader5-custom bash /config/start_mt5.sh
```

## Vérifier que le serveur RPyC tourne
```bash
docker exec metatrader5-custom ss -tuln | grep 8001
```

## Connecter MT5 à Fusion Markets
Accéder à l'interface web via le port récupéré ci-dessus
- Login : ton numéro de compte MT5
- Password : ton mot de passe MT5
- Server : FusionMarkets-Live

## Connecter le bot Python au réseau MT5
```bash
docker network create trading-network 2>/dev/null || true
docker network connect trading-network metatrader5-custom
```

## Versions importantes
- mt5linux : 1.0.3
- rpyc : 5.2.3
- numpy Wine Python : 1.26.4
