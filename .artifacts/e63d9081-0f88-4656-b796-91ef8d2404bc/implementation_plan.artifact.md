# Implementation Plan: Smart Pantry Manager

Design and build a Java-based Android application to help users track ingredients and suggest recipes based on a strict-matching rule.

## User Review Required

> [!IMPORTANT]
> **Database Choice:** I have selected **SQLite (via Room)** for local persistence, as it is robust, lifecycle-aware, and aligns well with standard Android module requirements.
>
> **Strict-Matching Logic:** To handle singular/plural and case sensitivity, the app will normalize ingredient names (lowercase and suffix stripping) during comparisons.

## Proposed Changes

### Configuration & Dependencies

#### [MODIFY] [build.gradle.kts](file:///C:/Users/jhunc/AndroidStudioProjects/MyApplication2/app/build.gradle.kts)
- Add dependencies for Room (Persistence), Navigation Components (Java), and Material Design.

---

### Data Layer (Java)

#### [NEW] [Ingredient.java](file:///C:/Users/jhunc/AndroidStudioProjects/MyApplication2/app/src/main/java/com/example/myapplication2/model/Ingredient.java)
- Entity representing a pantry item (name, quantity, unit, expiry).

#### [NEW] [Recipe.java](file:///C:/Users/jhunc/AndroidStudioProjects/MyApplication2/app/src/main/java/com/example/myapplication2/model/Recipe.java)
- Entity representing a recipe (name, preparation steps).

#### [NEW] [RecipeIngredient.java](file:///C:/Users/jhunc/AndroidStudioProjects/MyApplication2/app/src/main/java/com/example/myapplication2/model/RecipeIngredient.java)
- Entity linking recipes to their required ingredients and quantities.

#### [NEW] [PantryDatabase.java](file:///C:/Users/jhunc/AndroidStudioProjects/MyApplication2/app/src/main/java/com/example/myapplication2/db/PantryDatabase.java)
- Room database definition with pre-seeding logic for 15-20 initial recipes.

---

### Business Logic

#### [NEW] [MatchingEngine.java](file:///C:/Users/jhunc/AndroidStudioProjects/MyApplication2/app/src/main/java/com/example/myapplication2/logic/MatchingEngine.java)
- Implementation of the **Strict-Matching Rule**:
  - Compares required ingredients vs. pantry inventory.
  - Checks quantities and units.
  - Normalizes names to handle simple pluralization.

---

### UI Layer (Java)

#### [MODIFY] [MainActivity.java](file:///C:/Users/jhunc/AndroidStudioProjects/MyApplication2/app/src/main/java/com/example/myapplication2/MainActivity.java)
- Host activity with `BottomNavigationView`.

#### [NEW] [PantryFragment.java](file:///C:/Users/jhunc/AndroidStudioProjects/MyApplication2/app/src/main/java/com/example/myapplication2/ui/PantryFragment.java)
- Displays current ingredients in a `RecyclerView` with `IngredientAdapter`.

#### [NEW] [AddEditIngredientActivity.java](file:///C:/Users/jhunc/AndroidStudioProjects/MyApplication2/app/src/main/java/com/example/myapplication2/ui/AddEditIngredientActivity.java)
- Form for adding/editing items with input validation.

#### [NEW] [RecipeSuggestionsFragment.java](file:///C:/Users/jhunc/AndroidStudioProjects/MyApplication2/app/src/main/java/com/example/myapplication2/ui/RecipeSuggestionsFragment.java)
- Lists recipes that pass the strict-matching check.

#### [NEW] [RecipeDetailActivity.java](file:///C:/Users/jhunc/AndroidStudioProjects/MyApplication2/app/src/main/java/com/example/myapplication2/ui/RecipeDetailActivity.java)
- Displays full recipe details (ingredients + method).

#### [NEW] [SettingsFragment.java](file:///C:/Users/jhunc/AndroidStudioProjects/MyApplication2/app/src/main/java/com/example/myapplication2/ui/SettingsFragment.java)
- Toggle for expiry alerts and unit preferences.

---

## Verification Plan

### Automated Tests
- Unit test for `MatchingEngine` to verify strict matching (5/5 ingredients matches, 4/5 does not).
- Database migration/pre-seed tests.

### Manual Verification
- **CRUD cycle:** Add "Salt", edit quantity, delete "Salt", verify list updates.
- **Strict Matching:** Add all ingredients for a recipe -> verify it appears. Remove one -> verify it disappears.
- **Persistence:** Close and reopen the app to ensure data remains.
- **Validation:** Try to save an ingredient with an empty name or negative quantity.
