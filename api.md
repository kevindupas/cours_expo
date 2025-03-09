
# Documentation API - KeepNote Clone

Cette documentation présente les différentes routes API disponibles pour votre application KeepNote. Vous trouverez pour chaque endpoint des exemples de requêtes et de réponses.

## Table des matières

-   [Authentification](#authentification)
-   [Notes](#notes)
-   [Tâches](#t%C3%A2ches)
-   [Catégories](#cat%C3%A9gories)

## Authentification

### Inscription d'un nouvel utilisateur

```
POST /api/register

```

**Requête :**

```json
{
  "name": "Étudiant Test",
  "email": "etudiant@example.com",
  "password": "motdepasse123",
  "password_confirmation": "motdepasse123"
}

```

**Réponse (201 Created) :**

```json
{
  "access_token": "1|LKJDSFh387qkjhfsjk71Nzlkdfhsj8fdD...",
  "token_type": "Bearer",
  "user": {
    "id": 1,
    "name": "Étudiant Test",
    "email": "etudiant@example.com",
    "is_admin": false,
    "created_at": "2025-03-09T12:00:00.000000Z",
    "updated_at": "2025-03-09T12:00:00.000000Z"
  }
}

```

### Connexion

```
POST /api/login

```

**Requête :**

```json
{
  "email": "etudiant@example.com",
  "password": "motdepasse123"
}

```

**Réponse (200 OK) :**

```json
{
  "access_token": "1|LKJDSFh387qkjhfsjk71Nzlkdfhsj8fdD...",
  "token_type": "Bearer",
  "user": {
    "id": 1,
    "name": "Étudiant Test",
    "email": "etudiant@example.com",
    "is_admin": false,
    "created_at": "2025-03-09T12:00:00.000000Z",
    "updated_at": "2025-03-09T12:00:00.000000Z"
  }
}

```

### Mot de passe oublié

```
POST /api/forgot-password

```

**Requête :**

```json
{
  "email": "etudiant@example.com"
}

```

**Réponse (200 OK) :**

```json
{
  "message": "Email de réinitialisation envoyé"
}

```

### Réinitialisation du mot de passe

```
POST /api/reset-password

```

**Requête :**

```json
{
  "email": "etudiant@example.com",
  "token": "token_recu_par_email",
  "password": "nouveau_motdepasse",
  "password_confirmation": "nouveau_motdepasse"
}

```

**Réponse (200 OK) :**

```json
{
  "message": "Mot de passe réinitialisé avec succès"
}

```

### Déconnexion

```
POST /api/logout

```

**Headers :**

```
Authorization: Bearer {token}

```

**Réponse (200 OK) :**

```json
{
  "message": "Déconnexion réussie"
}

```

## Notes

### Récupérer toutes les notes

```
GET /api/notes

```

**Headers :**

```
Authorization: Bearer {token}

```

**Réponse (200 OK) :**

```json
{
  "data": [
    {
      "id": 1,
      "title": "Première note",
      "content": "<p>Contenu de ma première note</p>",
      "user_id": 1,
      "created_at": "2025-03-09T12:00:00.000000Z",
      "updated_at": "2025-03-09T12:00:00.000000Z",
      "categories": [
        {
          "id": 1,
          "name": "Personnel",
          "color": "#33FF57",
          "is_system": true,
          "user_id": null,
          "created_at": "2025-03-09T12:00:00.000000Z",
          "updated_at": "2025-03-09T12:00:00.000000Z",
          "pivot": {
            "note_id": 1,
            "category_id": 1
          }
        }
      ]
    }
  ]
}

```

### Créer une nouvelle note

```
POST /api/notes

```

**Headers :**

```
Authorization: Bearer {token}

```

**Requête :**

```json
{
  "title": "Ma nouvelle note",
  "content": "<p>Contenu de ma nouvelle note</p>",
  "categories": [1, 3]
}

```

**Réponse (201 Created) :**

```json
{
  "message": "Note créée avec succès",
  "data": {
    "id": 2,
    "title": "Ma nouvelle note",
    "content": "<p>Contenu de ma nouvelle note</p>",
    "user_id": 1,
    "created_at": "2025-03-09T12:30:00.000000Z",
    "updated_at": "2025-03-09T12:30:00.000000Z",
    "categories": [
      {
        "id": 1,
        "name": "Personnel",
        "color": "#33FF57",
        "is_system": true,
        "user_id": null,
        "created_at": "2025-03-09T12:00:00.000000Z",
        "updated_at": "2025-03-09T12:00:00.000000Z",
        "pivot": {
          "note_id": 2,
          "category_id": 1
        }
      },
      {
        "id": 3,
        "name": "Idées",
        "color": "#5733FF",
        "is_system": true,
        "user_id": null,
        "created_at": "2025-03-09T12:00:00.000000Z",
        "updated_at": "2025-03-09T12:00:00.000000Z",
        "pivot": {
          "note_id": 2,
          "category_id": 3
        }
      }
    ]
  }
}

```

### Récupérer les détails d'une note

```
GET /api/notes/{id}

```

**Headers :**

```
Authorization: Bearer {token}

```

**Réponse (200 OK) :**

```json
{
  "data": {
    "id": 2,
    "title": "Ma nouvelle note",
    "content": "<p>Contenu de ma nouvelle note</p>",
    "user_id": 1,
    "created_at": "2025-03-09T12:30:00.000000Z",
    "updated_at": "2025-03-09T12:30:00.000000Z",
    "categories": [...],
    "tasks": [...]
  }
}

```

### Mettre à jour une note

```
PUT /api/notes/{id}

```

**Headers :**

```
Authorization: Bearer {token}

```

**Requête :**

```json
{
  "title": "Ma note modifiée",
  "content": "<p>Contenu modifié</p>",
  "categories": [1, 2]
}

```

**Réponse (200 OK) :**

```json
{
  "message": "Note mise à jour avec succès",
  "data": {
    "id": 2,
    "title": "Ma note modifiée",
    "content": "<p>Contenu modifié</p>",
    "user_id": 1,
    "created_at": "2025-03-09T12:30:00.000000Z",
    "updated_at": "2025-03-09T13:00:00.000000Z",
    "categories": [...]
  }
}

```

### Supprimer une note

```
DELETE /api/notes/{id}

```

**Headers :**

```
Authorization: Bearer {token}

```

**Réponse (200 OK) :**

```json
{
  "message": "Note supprimée avec succès"
}

```

## Tâches

### Récupérer toutes les tâches

```
GET /api/tasks

```

**Headers :**

```
Authorization: Bearer {token}

```

**Paramètres de requête optionnels :**

```
?note_id=1       // Filtrer par note
?is_completed=0  // Filtrer par statut (0=non complété, 1=complété)

```

**Réponse (200 OK) :**

```json
{
  "data": [
    {
      "id": 1,
      "description": "Première tâche",
      "is_completed": false,
      "user_id": 1,
      "note_id": 2,
      "subtasks": [
        {
          "id": 1,
          "description": "Sous-tâche 1",
          "is_completed": false
        },
        {
          "id": 2,
          "description": "Sous-tâche 2",
          "is_completed": true
        }
      ],
      "created_at": "2025-03-09T13:30:00.000000Z",
      "updated_at": "2025-03-09T13:30:00.000000Z",
      "note": {
        "id": 2,
        "title": "Ma note modifiée"
      }
    }
  ]
}

```

### Créer une nouvelle tâche

```
POST /api/tasks

```

**Headers :**

```
Authorization: Bearer {token}

```

**Requête :**

```json
{
  "description": "Nouvelle tâche",
  "note_id": 2,
  "is_completed": false,
  "subtasks": [
    {
      "description": "Première sous-tâche",
      "is_completed": false
    },
    {
      "description": "Deuxième sous-tâche",
      "is_completed": false
    }
  ]
}

```

**Réponse (201 Created) :**

```json
{
  "message": "Tâche créée avec succès",
  "data": {
    "id": 2,
    "description": "Nouvelle tâche",
    "is_completed": false,
    "user_id": 1,
    "note_id": 2,
    "subtasks": [
      {
        "id": 1,
        "description": "Première sous-tâche",
        "is_completed": false
      },
      {
        "id": 2,
        "description": "Deuxième sous-tâche",
        "is_completed": false
      }
    ],
    "created_at": "2025-03-09T14:00:00.000000Z",
    "updated_at": "2025-03-09T14:00:00.000000Z"
  }
}

```

### Récupérer les détails d'une tâche

```
GET /api/tasks/{id}

```

**Headers :**

```
Authorization: Bearer {token}

```

**Réponse (200 OK) :**

```json
{
  "data": {
    "id": 2,
    "description": "Nouvelle tâche",
    "is_completed": false,
    "user_id": 1,
    "note_id": 2,
    "subtasks": [...],
    "created_at": "2025-03-09T14:00:00.000000Z",
    "updated_at": "2025-03-09T14:00:00.000000Z",
    "note": {
      "id": 2,
      "title": "Ma note modifiée"
    }
  }
}

```

### Mettre à jour une tâche

```
PUT /api/tasks/{id}

```

**Headers :**

```
Authorization: Bearer {token}

```

**Requête :**

```json
{
  "description": "Tâche modifiée",
  "is_completed": true,
  "note_id": 2,
  "subtasks": [
    {
      "id": 1,
      "description": "Première sous-tâche modifiée",
      "is_completed": true
    },
    {
      "id": 2,
      "description": "Deuxième sous-tâche",
      "is_completed": false
    }
  ]
}

```

**Réponse (200 OK) :**

```json
{
  "message": "Tâche mise à jour avec succès",
  "data": {
    "id": 2,
    "description": "Tâche modifiée",
    "is_completed": true,
    "user_id": 1,
    "note_id": 2,
    "subtasks": [...],
    "created_at": "2025-03-09T14:00:00.000000Z",
    "updated_at": "2025-03-09T14:30:00.000000Z"
  }
}

```

### Basculer l'état d'une tâche

```
PATCH /api/tasks/{id}/toggle

```

**Headers :**

```
Authorization: Bearer {token}

```

**Réponse (200 OK) :**

```json
{
  "message": "État de la tâche modifié avec succès",
  "data": {
    "id": 2,
    "description": "Tâche modifiée",
    "is_completed": false,
    "user_id": 1,
    "note_id": 2,
    "subtasks": [...],
    "created_at": "2025-03-09T14:00:00.000000Z",
    "updated_at": "2025-03-09T14:35:00.000000Z"
  }
}

```

### Supprimer une tâche

```
DELETE /api/tasks/{id}

```

**Headers :**

```
Authorization: Bearer {token}

```

**Réponse (200 OK) :**

```json
{
  "message": "Tâche supprimée avec succès"
}

```

### Ajouter une sous-tâche

```
POST /api/tasks/{id}/subtasks

```

**Headers :**

```
Authorization: Bearer {token}

```

**Requête :**

```json
{
  "description": "Nouvelle sous-tâche",
  "is_completed": false
}

```

**Réponse (200 OK) :**

```json
{
  "message": "Sous-tâche ajoutée avec succès",
  "data": {
    "id": 2,
    "description": "Tâche modifiée",
    "is_completed": false,
    "user_id": 1,
    "note_id": 2,
    "subtasks": [
      {
        "id": 1,
        "description": "Première sous-tâche modifiée",
        "is_completed": true
      },
      {
        "id": 2,
        "description": "Deuxième sous-tâche",
        "is_completed": false
      },
      {
        "id": 3,
        "description": "Nouvelle sous-tâche",
        "is_completed": false
      }
    ],
    "created_at": "2025-03-09T14:00:00.000000Z",
    "updated_at": "2025-03-09T14:40:00.000000Z"
  }
}

```

### Mettre à jour une sous-tâche

```
PUT /api/tasks/{id}/subtasks/{subtask_id}

```

**Headers :**

```
Authorization: Bearer {token}

```

**Requête :**

```json
{
  "description": "Sous-tâche modifiée",
  "is_completed": true
}

```

**Réponse (200 OK) :**

```json
{
  "message": "Sous-tâche mise à jour avec succès",
  "data": {
    "id": 2,
    "description": "Tâche modifiée",
    "is_completed": false,
    "user_id": 1,
    "note_id": 2,
    "subtasks": [
      {
        "id": 1,
        "description": "Première sous-tâche modifiée",
        "is_completed": true
      },
      {
        "id": 2,
        "description": "Sous-tâche modifiée",
        "is_completed": true
      },
      {
        "id": 3,
        "description": "Nouvelle sous-tâche",
        "is_completed": false
      }
    ],
    "created_at": "2025-03-09T14:00:00.000000Z",
    "updated_at": "2025-03-09T14:45:00.000000Z"
  }
}

```

### Supprimer une sous-tâche

```
DELETE /api/tasks/{id}/subtasks/{subtask_id}

```

**Headers :**

```
Authorization: Bearer {token}

```

**Réponse (200 OK) :**

```json
{
  "message": "Sous-tâche supprimée avec succès",
  "data": {
    "id": 2,
    "description": "Tâche modifiée",
    "is_completed": false,
    "user_id": 1,
    "note_id": 2,
    "subtasks": [
      {
        "id": 1,
        "description": "Première sous-tâche modifiée",
        "is_completed": true
      },
      {
        "id": 3,
        "description": "Nouvelle sous-tâche",
        "is_completed": false
      }
    ],
    "created_at": "2025-03-09T14:00:00.000000Z",
    "updated_at": "2025-03-09T14:50:00.000000Z"
  }
}

```

## Catégories

### Récupérer toutes les catégories

```
GET /api/categories

```

**Headers :**

```
Authorization: Bearer {token}

```

**Réponse (200 OK) :**

```json
{
  "data": [
    {
      "id": 1,
      "name": "Personnel",
      "color": "#33FF57",
      "is_system": true,
      "user_id": null,
      "created_at": "2025-03-09T12:00:00.000000Z",
      "updated_at": "2025-03-09T12:00:00.000000Z"
    },
    {
      "id": 2,
      "name": "Travail",
      "color": "#FF5733",
      "is_system": true,
      "user_id": null,
      "created_at": "2025-03-09T12:00:00.000000Z",
      "updated_at": "2025-03-09T12:00:00.000000Z"
    },
    {
      "id": 4,
      "name": "Ma catégorie",
      "color": "#33B5FF",
      "is_system": false,
      "user_id": 1,
      "created_at": "2025-03-09T15:00:00.000000Z",
      "updated_at": "2025-03-09T15:00:00.000000Z"
    }
  ]
}

```

### Créer une nouvelle catégorie

```
POST /api/categories

```

**Headers :**

```
Authorization: Bearer {token}

```

**Requête :**

```json
{
  "name": "Nouvelle catégorie",
  "color": "#9C27B0"
}

```

**Réponse (201 Created) :**

```json
{
  "message": "Catégorie créée avec succès",
  "data": {
    "id": 5,
    "name": "Nouvelle catégorie",
    "color": "#9C27B0",
    "is_system": false,
    "user_id": 1,
    "created_at": "2025-03-09T15:30:00.000000Z",
    "updated_at": "2025-03-09T15:30:00.000000Z"
  }
}

```

### Mettre à jour une catégorie

```
PUT /api/categories/{id}

```

**Headers :**

```
Authorization: Bearer {token}

```

**Requête :**

```json
{
  "name": "Catégorie modifiée",
  "color": "#673AB7"
}

```

**Réponse (200 OK) :**

```json
{
  "message": "Catégorie mise à jour avec succès",
  "data": {
    "id": 5,
    "name": "Catégorie modifiée",
    "color": "#673AB7",
    "is_system": false,
    "user_id": 1,
    "created_at": "2025-03-09T15:30:00.000000Z",
    "updated_at": "2025-03-09T15:45:00.000000Z"
  }
}

```

### Supprimer une catégorie

```
DELETE /api/categories/{id}

```

**Headers :**

```
Authorization: Bearer {token}

```

**Réponse (200 OK) :**

```json
{
  "message": "Catégorie supprimée avec succès"
}

```

----------

## Notes importantes

1.  Toutes les requêtes API (sauf l'authentification) nécessitent l'envoi du token d'authentification dans l'en-tête `Authorization`.
    
2.  Pour les tâches, le champ `note_id` est facultatif. Si vous ne spécifiez pas de note, la tâche sera créée de manière indépendante.
    
3.  Les sous-tâches sont stockées en JSON dans la table des tâches. Vous n'avez pas besoin de créer une table supplémentaire.
    
4.  Vous ne pouvez pas modifier ou supprimer les catégories système (`is_system = true`).
    
5.  Toutes les dates sont au format ISO 8601 (UTC).
