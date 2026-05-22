# Deep Link Android

```kt
private fun handleDeepLink(intent: Intent?) {
        if (intent?.action != Intent.ACTION_VIEW) return

        val uri = intent.data ?: return
        if (uri.isRecipeDeepLink()) {
            val foodId = uri.getRecipeFoodId()
            if (foodId.isNullOrBlank()) {
                showRecipe()
            } else {
                showRecipeDetail(foodId)
            }
        }
    }

    private fun Uri.isRecipeDeepLink(): Boolean {
        return scheme == DEEP_LINK_SCHEME && host == DEEP_LINK_RECIPE_HOST
    }

    private fun Uri.getRecipeFoodId(): String? {
        return pathSegments.firstOrNull()
    }

```
- Nó lấy **pathSegment** từ trong **intent**

- Vì deep link đi vào app thông qua Intent, còn pathSegment là phần con nằm bên trong Uri của Intent.

Nói chính xác hơn:

```kt
Intent
 └── data: Uri
      ├── scheme = roadmap
      ├── host = recipe
      └── pathSegments = ["123", "pending"]

Với link:

roadmap://recipe/123/pending

Android tạo một Intent kiểu:


Intent(
    action = Intent.ACTION_VIEW,
    data = Uri.parse("roadmap://recipe/123/pending")
)
```
Nên trong app bạn lấy như này:
```kt
val uri = intent.data
val recipeId = uri?.pathSegments?.getOrNull(0)
val status = uri?.pathSegments?.getOrNull(1)
```