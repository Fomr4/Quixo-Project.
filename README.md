# Quixo-Project

## Description
Ce projet implémente une version automatisée du jeu **Quixo**, où le joueur humain affronte un joueur robotisé. Le but est de créer une ligne de cinq cubes avec sa marque (`X` pour le joueur humain) sur un plateau de 5x5.

## Prérequis
- **Python** (version 3.7 ou plus récente)
- **Bibliothèques Python** :
  - `requests` pour communiquer avec le serveur de jeu

## Installation

1. **Cloner le Dépôt** :
   Exécutez cette commande pour cloner le dépôt et naviguer dans le dossier du projet :
   ```bash
   git clone https://github.com/Form4/Quixo-Project.git
   cd Quixo-Project

pip install requests

git add main.py quixo.py api.py
git commit -m ""[Uplfrom quixo import formater_le_jeu, choisir_un_coup, interpréter_la_commande
from api import initialiser_partie, jouer_un_coup

if __name__ == "__main__":
    args = interpréter_la_commande()
    idul = args.idul
    secret = "80a0a0d2-059d-4539-9d53-78b3f6045943"  # Remplace par ton propre jeton si besoin

    try:
        id_partie, joueurs, etat_plateau = initialiser_partie(idul, secret)
        print(formater_le_jeu(joueurs, etat_plateau))

        while True:
            try:
                origine, direction = choisir_un_coup()
                id_partie, joueurs, etat_plateau = jouer_un_coup(id_partie, origine, direction, idul, secret)
                print(formater_le_jeu(joueurs, etat_plateau))
            except StopIteration as gagnant:
                print(f"Partie terminée! Le gagnant est {gagnant}")
                break

    except PermissionError as e:
        print(f"Erreur d'authentification : {e}")
    except RuntimeError as e:
        print(f"Erreur de jeu : {e}")
    except ConnectionError as e:
        print(f"Erreur de connexion : {e}")
import argparse


def formater_entete(joueurs):
    return f"Légende:\n   X={joueurs[0]}\n   O={joueurs[1]}"

def formater_le_damier(plateau):
    lignes = []
    lignes.append("   -------------------")
    for i, ligne in enumerate(plateau, start=1):
        lignes.append(f"{i} | {' | '.join(ligne)} |")
        if i < len(plateau):
            lignes.append("  |---|---|---|---|---|")
    lignes.append("--|---|---|---|---|---|")
    lignes.append("  | 1   2   3   4   5 |")
    return "\n".join(lignes)

def formater_le_jeu(joueurs, plateau):
    entete = formater_entete(joueurs)
    damier = formater_le_damier(plateau)
    return f"{entete}\n{damier}"

def choisir_un_coup():
    origine = list(map(int, input("Donnez la position d'origine du cube (x,y) : ").split(',')))
    direction = input("Quelle direction voulez-vous insérer? ('haut', 'bas', 'gauche', 'droite') : ")
    return origine, direction

def interpréter_la_commande():
    parser = argparse.ArgumentParser(description="Quixo")
    parser.add_argument("idul", help="IDUL du joueur")
    return parser.parse_args()
import requests

def initialiser_partie(idul, secret):
    url = "https://pax.ulaval.ca/quixo/api/a24/partie/"
    response = requests.post(url, auth=(idul, secret))
    if response.status_code == 200:
        data = response.json()
        return data['id'], data['joueurs'], data['état']
    elif response.status_code == 401:
        raise PermissionError(response.json()['message'])
    elif response.status_code == 406:
        raise RuntimeError(response.json()['message'])
    else:
        raise ConnectionError("Erreur de connexion.")

def jouer_un_coup(id_partie, origine, direction, idul, secret):
    url = f"https://pax.ulaval.ca/quixo/api/a24/partie/{id_partie}/"
    response = requests.put(url, auth=(idul, secret), json={"origine": origine, "direction": direction})
    if response.status_code == 200:
        data = response.json()
        if data['gagnant']:
            raise StopIteration(data['gagnant'])
        return data['id'], data['joueurs'], data['état']
    elif response.status_code == 401:
        raise PermissionError(response.json()['message'])
    elif response.status_code == 406:
        raise RuntimeError(response.json()['message'])
    else:
        raise ConnectionError("Erreur de connexion.")oading main.py…]()


        git push origin main



git bundle create quixo_project.bundle --all




        

