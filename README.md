# Movie Data Scraping and Analysis
## Objective
The goal of this project is to extract detailed and structured information about movies from IMDb — a widely trusted and comprehensive source of movie data. Specifically, we aim to gather data such as the movie title, genre, release year, and IMDb rating. Additionally, we will enrich this dataset by integrating it with a supplementary movie dataset from Kaggle, creating a comprehensive data source for further analysis.

This structured dataset will then be stored in a database, enabling easy access and manipulation. By organizing and structuring this data, we unlock its potential for applications such as building recommendation systems, conducting trend analyses, and creating visualizations to understand movie ratings and genre popularity over time.

## Initial Question
"How can we store and analyze movie ratings over time, and are there any correlations between movie genres and their ratings?"

## Methodology
**Data Extraction**:

To begin, we initiated HTTP requests to IMDb’s Top 1000 Movies pages using the `requests` library. Since IMDb paginates its lists, we looped through the pages in increments of 250, fetching data from each page. To mimic a browser request and avoid being blocked, we included a custom user-agent header in the requests. The responses returned the HTML content of each page, which we processed in the subsequent steps.

 ### Data Extraction Process

1. **Scraping Data from IMDb**:
    - We used the `BeautifulSoup` library to scrape movie details from IMDb.
    - The raw data was initially stored in **JSON format** for structured access.

2. **Extracting Embedded JSON-LD Data**:
    - JSON-LD is a structured data format embedded in the HTML of IMDb pages, containing rich metadata about the movies.
    - We utilized regular expressions to locate the `<script>` tag containing this JSON-LD data and parsed it into a Python dictionary.
    - This method allowed us to efficiently access detailed movie information.

3. **Extracting Key Movie Details**:
    - From the parsed JSON-LD data, we extracted the following key details for each movie:
        - Title
        - Description
        - URL
        - IMDb Rating
        - Content Rating
        - Genre
        - Duration
    - For any missing fields in the source data, we assigned default values like ‘N/A’ to maintain a consistent structure across all records.

4. **Storing Data**:
    - Each movie's details were stored as individual records in a Python list, forming a comprehensive collection of information for the Top 1000 Movies.
    - Once all the data was gathered, we saved it into a **JSON file** named `movies_data.json`. This file retained the hierarchical structure of the data, which made it suitable for further processing.
    
5. **Data Conversion for Analysis**:
    - For easier analysis and visualization, we converted the JSON data into a Pandas DataFrame and exported it as a **CSV file** named `movies_data.csv`.
    - This provided us with both **JSON** and **CSV** formats for further exploration and analysis.


### Data Intergration and Cleaning

This phase of the project focused on integrating and cleaning data by merging the IMDb Top 1000 Movies dataset with a larger dataset of over 1 million movies from Kaggle. The integration process was carried out in the following steps:
link for kaggle dataset:

1. **Standardizing Column Names**:
    - Both datasets were reviewed, and column names were standardized to ensure consistency across the two datasets.

2. **Merging Datasets**:
    - The datasets were merged using the `title` column as the common key. This enriched the IMDb dataset with additional movie details from the Kaggle dataset, such as:
        - Production information
        - Cast
        - Budget
        - Revenue

3. **Data Cleaning**:
    - Removed duplicates and handled missing values to ensure data consistency.
    - Unnecessary columns were dropped to streamline the dataset.
    - Duplicate entries were removed based on movie titles to ensure uniqueness.
    - The `duration` column was converted into a standardized time format (HH:MM:SS).
    - Rows with missing values were eliminated to ensure a clean and complete dataset.

The resulting dataset is now enriched, standardized, and ready for further analysis or visualization, providing a comprehensive resource for exploring movie trends and metrics.

### Database Setup:
In this stage of the project, we focused on setting up a MySQL database to store the structured movie data. The first step was to establish a connection to the MySQL database using the `pymysql` library. With the connection in place, we proceeded to create multiple tables to store different types of movie-related information. These tables included:

- **Movies**: Stores basic movie details like title, rating, release date, and budget.
- **Genres**: Stores movie genres such as action, comedy, drama, etc.
- **Movie_Genre**: A join table to associate movies with their respective genres.
- **Cast**: Stores information about actors and actresses.
- **Movie_Cast**: A join table to link movies to their cast members.
- **Directors**: Stores director information.
- **Movie_Director**: A join table to link movies with their directors.
- **Writers**: Stores information about the writers of the movie.
- **Movie_Writer**: A join table to link movies with their writers.
- **Movie_Production**: Stores production company information.

Each table was created using SQL `CREATE TABLE` commands, ensuring the necessary fields, relationships, and foreign key constraints were included. These constraints were put in place to maintain data integrity across the different tables.

**Advantages of Database Setup**:
- **Data Integrity**: The use of foreign key constraints between tables ensures that data is consistent and accurate, preventing orphan records and ensuring relationships between entities (e.g., movies and genres, cast members, directors) are properly maintained.
- **Scalability**: By structuring the data in a relational database, it becomes easier to scale the project. New data or entities can be added without disrupting the existing data structure, making it a flexible solution for future data growth.
- **Efficient Querying**: Storing movie data in a structured database allows for complex querying and retrieval of specific information (e.g., fetching movies by genre, rating, or director). This enables efficient data analysis and supports further exploration of movie trends.
- **Data Centralization**: By storing all the data in a central database, it is easier to manage and maintain. All data related to movies, genres, cast, etc., is housed in one location, making it easier to access and update.

### Data Insertion:
Once the database structure was established, we moved on to inserting the cleaned movie data into the relevant tables. The insertion process involved using several functions to populate each table:

- **insert_movies**: This function inserted basic movie details, such as the movie title, IMDb rating, release date, and budget, into the `Movies` table.
- **insert_genres**: This function inserted movie genres into the `Genres` table.
- **insert_cast**: This function inserted cast members into the `Cast` table.
- **insert_directors**: This function inserted director information into the `Directors` table.
- **insert_writers**: This function inserted writer information into the `Writers` table.
- **insert_production**: This function inserted production company details into the `Movie_Production` table.

Additionally, the **join tables** (such as `Movie_Genre`, `Movie_Cast`, `Movie_Director`, `Movie_Writer`) were used to create relationships between the movies and their respective genres, cast members, directors, writers, and production companies. These functions ensured that each genre, cast, director, writer, and production company was inserted uniquely, maintaining the integrity of the data.

**Advantages of Data Insertion**:
- **Data Accuracy**: Using structured functions to insert data into specific tables ensures that each type of data (e.g., genres, cast, directors) is inserted correctly and avoids redundancy or errors.
- **Relationships Maintenance**: By using join tables to link movies with their respective genres, cast, and production teams, the relationships between these entities are accurately recorded and can be efficiently queried or updated.
- **Data Normalization**: The insertion process ensures that data is normalized, meaning that movie details are stored separately from other related data (such as cast or genre). This reduces duplication and makes it easier to update or modify data without affecting other tables.
- **Efficient Data Management**: With the database fully populated, it becomes easy to manage and update movie data. This structured approach makes future modifications, such as adding new movies or updating cast and production details, quick and straightforward.

After executing the insertion queries, the MySQL database was successfully populated with structured movie data. The relationships between entities (e.g., movies and their genres, cast members, directors) were maintained through foreign key references. This ensures that the data remains consistent and can be easily queried for further analysis, visualization, or reporting.





### Testing the database
#### testing the relationships between tables
Check if the relationships between tables are working by performing some JOIN queries. For example, to check which movies belong to a particular genre, you can perform a join between Movies and Movie_Genre:

<img width="209" alt="sql query" src="https://github.com/user-attachments/assets/bf3a1627-a639-4264-90ff-37f26bbfc56c">

- Result

  <img width="157" alt="result 1" src="https://github.com/user-attachments/assets/53f1fd63-762c-4247-8385-0c57e0b99dc1">

  #### Check for Missing or Null Values
  Ensure that there are no missing or null values in the important fields (like movie_id, genre_name, cast_name, etc.).
  - Query
  
  <img width="238" alt="query 2" src="https://github.com/user-attachments/assets/c48d705e-6523-46aa-b547-a554ab963a62">


  - Result
  
  <img width="570" alt="result" src="https://github.com/user-attachments/assets/47a63c7c-f74a-464d-8c62-1eb005984290">

  The result returned show that there is no missing or null values fro the dataset in the database



