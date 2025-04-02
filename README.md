import pygame
import random
import sys

# Initialisation de pygame
pygame.init()

# Configuration de la fenêtre
largeur, hauteur = 800, 600
fenetre = pygame.display.set_mode((largeur, hauteur))
pygame.display.set_caption("Évite les obstacles !")

# Couleurs
BLANC = (255, 255, 255)
ROUGE = (255, 0, 0)
BLEU = (0, 0, 255)

# Paramètres du joueur
joueur_taille = 50
joueur_x = largeur // 2 - joueur_taille // 2
joueur_y = hauteur - joueur_taille - 20
joueur_vitesse = 5

# Obstacles
obstacles = []
obstacle_vitesse = 3
obstacle_frequence = 25  # Plus petit = plus fréquent

# Score
score = 0
police = pygame.font.SysFont(None, 36)

# Horloge pour contrôler la vitesse de la boucle
horloge = pygame.time.Clock()

def afficher_score():
    texte = police.render(f"Score: {score}", True, BLANC)
    fenetre.blit(texte, (10, 10))

def nouveau_obstacle():
    x = random.randint(0, largeur - joueur_taille)
    y = -joueur_taille
    obstacles.append([x, y])

# Boucle principale
running = True
while running:
    fenetre.fill((0, 0, 0))  # Fond noir

    # Gestion des événements
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # Déplacement du joueur
    touches = pygame.key.get_pressed()
    if touches[pygame.K_LEFT] and joueur_x > 0:
        joueur_x -= joueur_vitesse
    if touches[pygame.K_RIGHT] and joueur_x < largeur - joueur_taille:
        joueur_x += joueur_vitesse

    # Génération d'obstacles
    if random.randint(1, obstacle_frequence) == 1:
        nouveau_obstacle()

    # Mouvement des obstacles
    for obstacle in obstacles[:]:
        obstacle[1] += obstacle_vitesse
        pygame.draw.rect(fenetre, ROUGE, (obstacle[0], obstacle[1], joueur_taille, joueur_taille))

        # Collision
        if (joueur_x < obstacle[0] + joueur_taille and
            joueur_x + joueur_taille > obstacle[0] and
            joueur_y < obstacle[1] + joueur_taille and
            joueur_y + joueur_taille > obstacle[1]):
            running = False  # Fin du jeu

        # Supprimer les obstacles hors écran + incrémenter le score
        if obstacle[1] > hauteur:
            obstacles.remove(obstacle)
            score += 1

    # Dessiner le joueur
    pygame.draw.rect(fenetre, BLEU, (joueur_x, joueur_y, joueur_taille, joueur_taille))

    # Afficher le score
    afficher_score()

    pygame.display.update()
    horloge.tick(60)  # 60 FPS

pygame.quit()
sys.exit()
