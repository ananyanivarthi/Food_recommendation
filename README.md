# Recipe Recommendation System

## 1. Approach

For this recipe recommendation system, we used a **TF-IDF (Term Frequency-Inverse Document Frequency)** model along with **cosine similarity** to recommend recipes based on user preferences. Here's an overview of the approach:

- **Dataset**: We use a dataset Recipe1M+ Dataset[Provides recipe details, ingredients, and user reviews.
Download also available here: https://www.kaggle.com/datasets/saldenisov/recipenlg/data]
containing recipes, including their ingredients, directions, and other information such as dietary restrictions (NER), and recipe links. We clean and preprocess this dataset to remove null values and unnecessary columns.
  
- **Text Preprocessing**: 
  - We preprocess the recipe text (ingredients + directions) by converting everything to lowercase and removing punctuation. This ensures that our model can focus on the essential components without being affected by case or punctuation differences.
  
- **TF-IDF Vectorization**:
  - The **TF-IDF vectorizer** is used to transform the combined text of ingredients and directions into numerical form, representing the importance of each word in the context of the dataset.

- **Cosine Similarity**:
  - Once the recipes are vectorized, we calculate **cosine similarity** between the user's input and the recipes in the dataset to identify the most relevant recipes based on user preferences.

- **Filtering Based on Dietary Restrictions**:
  - The system allows users to input dietary restrictions such as "vegetarian", "gluten-free", "dairy-free", and "nut-free". Recipes are filtered based on the presence of ingredients that contradict the user's dietary preferences. 

- **User Input**:
  - The Streamlit interface allows users to input dietary restrictions, cuisine preferences, available ingredients, preferred cooking time, difficulty level, and meal type.

- **Recommendation Logic**:
  - The user’s preferences are dynamically combined to form a query that is transformed into a TF-IDF vector. The cosine similarity between this query and the recipe database is calculated, and the top N most similar recipes are recommended. These recommendations are then filtered according to the dietary restrictions provided by the user.

## 2. Challenges and Solutions

- **Challenge 1: Handling User Preferences**:
  - **Solution**: The user input, such as ingredients and dietary restrictions, was dynamically combined into a text query. Each of these inputs was preprocessed and converted into a vector for calculating similarity. This ensured that the model can evaluate the relevance of recipes based on different factors simultaneously.
  
- **Challenge 2: Filtering Recipes Based on Dietary Restrictions**:
  - **Solution**: Filtering based on dietary restrictions required identifying certain keywords (e.g., "chicken", "wheat") and ensuring that the dataset could be filtered without causing performance issues. The approach involved using `str.contains()` to match relevant keywords in the ingredients field. To improve efficiency, these checks were performed after the initial recommendation was made.

- **Challenge 3: Dataset Size**:
  - **Solution**: The dataset was large (100,000+ recipes), and calculating cosine similarity for each user query against all recipes can be computationally expensive. To address this, we focused on optimizing the TF-IDF vectorization and used efficient pandas operations for filtering.

- **Challenge 4: Scalability**:
  - **Solution**: Given the size of the dataset, the system could become slow when computing similarity for many users. For larger systems, an improvement could be precomputing and storing cosine similarities or using a more sophisticated recommendation algorithm like **k-Nearest Neighbors** (k-NN) or **Collaborative Filtering** to enhance performance.

## 3. Ideas for Improvement

- **User Feedback Loop**:
  - One way to improve the recommendation system would be to incorporate **user feedback**. For instance, after users try recipes, they could rate them, and this feedback could be used to refine recommendations further (e.g., incorporating a collaborative filtering approach alongside content-based filtering).

- **Incorporating Nutritional Information**:
  - If additional nutritional information (e.g., calories, macronutrients) was available in the dataset, the system could make more informed recommendations by considering the nutritional preferences of users (e.g., high-protein, low-carb).

- **More Detailed Dietary Restrictions**:
  - Currently, the dietary restrictions are checked by matching keywords (e.g., "vegetarian", "gluten-free"). A more refined approach could be taken by using an ontology or a more structured dataset of ingredients and their classifications to ensure more accurate filtering.

- **Enhanced Query Representation**:
  - Instead of using the simple text-based user input query, advanced models like **BERT** or **Doc2Vec** could be used to represent user preferences and recipes in a richer vector space. These models are more adept at capturing contextual meaning, especially when the user query includes multiple constraints (ingredients, restrictions, etc.).

- **Faster Recommendation Computation**:
  - With a large number of recipes, calculating cosine similarity for each user query against the entire dataset could become slow. Using **approximate nearest neighbor** (ANN) methods or employing models that can index large datasets (e.g., FAISS, HNSW) would significantly improve the response time.

- **Adding More Interactive Features**:
  - Users could input additional parameters such as preferred cooking methods (e.g., baking, grilling), ingredients to avoid (e.g., allergies), and even ingredients that they want to include in the recipe but are missing from the dataset. These parameters could be integrated into the recommendation engine to make the system more flexible.


