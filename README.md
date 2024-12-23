
# Hook `useMutation`

## Prérequis
- **Node** >= 20
- **Vite** + React + TypeScript
- **Axios** (pour obtenir directement les erreurs HTTP > 400 sous forme d'erreurs)

## Fonctionnalités attendues

### Configurations
- **Partager une configuration globale** entre tous les hooks (exemple : `baseUrl` pour le test).

### Méthodes autorisées
- Accepte uniquement les méthodes **POST**, **PUT**, et **PATCH**.
- La méthode par défaut est **POST**.

### Callbacks
- Possibilité de fournir des callbacks **optionnels** : `onSuccess` et `onError`.

### To promise
- Possibilité de transformer le retour en promesse afin de traiter le résultat de façon "synchrone"

### Abort
- Possibilité d'annuler ( abort ) un call HTTP en cours

### Typage
- **Typage** pour :
  - Les données en retour (`data`).
  - Les données envoyées dans le corps de la requête (`body`).

---

## API et exemples

```typescript
const [mutation, { isLoading }] = useMutation({
  url: '/posts',
  method: 'POST', // Méthode par défaut
  onSuccess(data) {
    console.log("onSuccess", { data });
  },
  onError(error) {
    console.log("onError", { error });
  },
});

// Déclenchement classique
const handleSubmit = () => {
  mutation({
    title: 'foo',
    body: 'bar',
  });
};

// toPromise
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

// Abort
const handleSubmit = () => {
  const triggerResult = mutation({
    title: 'foo',
    body: 'bar',
  });

  triggerResult.abort();
};
