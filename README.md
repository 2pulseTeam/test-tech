# 📌 Hook `useMutation`

## 🏆 Objectif

L'objectif de ce test est d'implémenter un **hook React personnalisé** nommé `useMutation` permettant d'effectuer des requêtes HTTP (POST, PUT, PATCH) avec **Axios**.

Ce hook doit être **flexible, typé en TypeScript** et proposer des fonctionnalités avancées telles que **la gestion d'état, l'annulation de requête et un mode promesse**.

---

## 📋 Prérequis

Avant de commencer, assurez-vous d'avoir l'environnement suivant :

- **Node.js** >= 20
- **Vite** (pour le bundler)
- **React** avec **TypeScript**
- **Axios** (pour gérer les requêtes HTTP et les erreurs)

---

## 🎯 Fonctionnalités attendues

### ✅ 1. Gestion des méthodes HTTP

- Le hook doit **uniquement** supporter les méthodes suivantes (controle Typescript uniquement) :
  - **POST**
  - **PUT**
  - **PATCH**
- La méthode **POST** est celle utilisée par défaut si aucune méthode n'est spécifiée.

---

### 🎯 2. Gestion des callbacks

Le hook doit permettre l'exécution de **callbacks optionnels** afin de gérer les réponses et erreurs :

- `onSuccess(data: Data)`: Callback déclenché en cas de succès de la requête, recevant les données retournées.
- `onError(error: any)`: Callback déclenché si la requête échoue, recevant l'erreur retournée par Axios.

Ces callbacks permettent une gestion flexible des réponses côté application.

---

### ⏳ 3. Gestion de l'état de chargement (`isLoading`)

- Le hook doit exposer une variable `isLoading` indiquant si une requête est en cours.
- Cette valeur passe à `true` au début de l'exécution de la requête et redevient `false` une fois terminée (succès ou erreur).

---

### 🔄 4. Mode `toPromise` (Retour en promesse)

Le hook doit offrir un moyen de gérer les requêtes de manière **synchrone** grâce à **`async/await`**.  

👉 Cela permet aux développeurs de traiter les mutations de manière plus **déclarative**, sans dépendre uniquement des callbacks.

---

### Exemples d'utilisation de `useMutation` :

```typescript
interface Data {
  id: number;
  title: string;
  body: string;
}

interface Body {
  title: string;
  body: string;
}

// Définition du hook avec typage des données en entrée et en sortie
const [mutation, { isLoading }] = useMutation<Data, Body>({
  url: '/posts',
  method: 'POST',
  onSuccess(data) {
    console.log("onSuccess", { data });
  },
  onError(error) {
    console.log("onError", { error });
  },
});

// Utilisation classique avec les callbacks
const handleSubmit = () => {
  mutation({
    title: 'foo',
    body: 'bar',
  });
};

// Utilisation en promesse
const handleSubmit = async () => {
  try {
    const data = await mutation({
      title: 'foo',
      body: 'bar',
    }).toPromise();

    console.log({ data });
  } catch (error) {
    console.error({ error });
  }
};

// Possibilité d'interrompre de la requête en cours
const handleSubmit = () => {
  const triggerResult = mutation({
    title: 'foo',
    body: 'bar',
  });

  triggerResult.abort();
};
```
