# Natural_Language_to_SQL_Generator
 Talk to The Data: Building a Natural Language to SQL Generator


# Text-to-SQL: Bridging the Gap Between Human Language and Databases


Text-to-SQL, also known as Natural Language to SQL (NL2SQL), is a rapidly evolving technology that translates natural, everyday language into Structured Query Language (SQL) commands. This innovative approach empowers users to interact with and retrieve data from databases simply by asking questions in plain English, eliminating the need for specialized knowledge of complex SQL syntax.

At its core, Text-to-SQL acts as an intelligent translator. It leverages the power of artificial intelligence, particularly **Natural Language Processing (NLP)** and sophisticated **AI models**, to understand the user's intent and generate the corresponding SQL query. This process allows individuals without a technical background to explore and analyze data, thereby democratizing data access within an organization.


![alt text](<Model workflow-1.png>)

## How It Works: From a Simple Question to a Complex Query

The conversion of a user's question into an executable SQL query involves a multi-step process:

1.  **Natural Language Understanding (NLU):** The system first analyzes the user's input to decipher its meaning. This involves identifying key entities (like specific columns or tables), the relationships between them, and the user's ultimate goal (e.g., to filter, aggregate, or sort data).

2.  **Schema Linking:** Once the intent is understood, the system maps the identified entities from the natural language question to the specific tables and columns within the database's schema. This is a critical step to ensure the generated query is accurate and relevant to the available data structure.

3.  **SQL Generation:** With the user's intent and the relevant database schema components identified, the AI model constructs the appropriate SQL query. This can range from a simple `SELECT` statement to a complex query involving multiple `JOIN`s, `WHERE` clauses, and aggregate functions.

4.  **Query Execution and Response:** The generated SQL query is then executed against the database. The retrieved data is presented back to the user in a clear and understandable format, often as a table, chart, or a natural language summary.


## Key Applications and Use Cases

The ability to query databases using natural language has a wide array of applications across various industries:

* **Business Intelligence (BI) and Analytics:** Business analysts and decision-makers can quickly get answers to their data-driven questions without relying on data scientists or IT professionals. This accelerates the pace of analysis and reporting.
* **Data Exploration:** For both technical and non-technical users, Text-to-SQL provides an intuitive way to explore large and unfamiliar datasets, uncover insights, and formulate more specific data requests.
* **Customer Support:** Chatbots and virtual assistants integrated with Text-to-SQL can provide customers with real-time information by querying relevant databases based on their questions.
* **E-commerce:** Users can search for products using natural language filters and criteria, which are then translated into database queries to retrieve the most relevant results.

## The Advantages and Challenges

**Benefits:**

* **Increased Accessibility:** It breaks down the barrier to data, allowing a broader range of users to perform data analysis.
* **Improved Efficiency:** It significantly speeds up the process of data retrieval and report generation.
* **Reduced Reliance on Technical Experts:** It frees up data professionals from writing routine queries, allowing them to focus on more complex tasks.

**Challenges:**

* **Ambiguity of Natural Language:** Human language is often imprecise and context-dependent, which can lead to misinterpretation by the AI and the generation of incorrect queries.
* **Complex Database Schemas:** Large and intricately designed databases can pose a significant challenge for the AI to navigate and understand the relationships between numerous tables and columns.
* **Handling Complex Queries:** While proficient at generating simpler queries, Text-to-SQL systems can sometimes struggle with highly complex requests that require deep domain knowledge and intricate logic.

Despite these challenges, the field of Text-to-SQL is continuously advancing, with ongoing research focused on improving the accuracy, robustness, and capabilities of these powerful systems. As AI models become more sophisticated, Text-to-SQL is poised to become an indispensable tool for seamless and intuitive data interaction.


**Appoach**
![alt text](<Process flow-1.png>)


**Data Source**
Data is generaged from mocKaroo http://mockaroo.com
A synthetic ecommarce dataset created for the perpose of testing the model.
Three tables are generated containing data as follows
1 - Customers
2 - Products
3 - Orders

**Database Diagram:**

![alt text](<Data Diagram-1.png>)

## **Prompt:**

### **ROLE**

You are an expert-level SQLite Database Engineer specializing in Natural Language to SQL (NL2SQL) translation. Your sole function is to convert user questions written in plain English into accurate, efficient, and syntactically correct SQLite queries based on a fixed database schema.

-----

### **CONTEXT**

You are the core translation engine for a business intelligence dashboard. This tool allows non-technical employees to query the company's e-commerce database using natural language. The database dialect is always **SQLite**. Your responses will be executed directly on the database.

The database consists of the following three tables:

**`customers` table:**

```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(50),
    phone_number VARCHAR(50),
    address VARCHAR(50),
    city VARCHAR(50),
    country VARCHAR(50),
    postal_code VARCHAR(50),
    loyalty_points INT
);
```

**`products` table:**

```sql
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name TEXT,
    description TEXT,
    price DECIMAL(10,2),
    discount_percentage DECIMAL(5,2),
    category VARCHAR(50),
    brand TEXT,
    stock_quantity INT,
    color VARCHAR(50),
    size VARCHAR(20),
    weight DECIMAL(5,2),
    dimensions TEXT,
    release_date DATE,
    rating DECIMAL(3,1),
    reviews_count INT,
    seller_name TEXT,
    seller_rating DECIMAL(3,1),
    seller_reviews_count INT,
    shipping_method VARCHAR(20),
    shipping_cost DECIMAL(6,2)
);
```

**`orders` table:**

```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    product_id INT,
    quantity INT,
    unit_price DECIMAL(10,2),
    total_price DECIMAL(10,2),
    order_date DATE,
    shipping_address VARCHAR(255),
    payment_method VARCHAR(20),
    status VARCHAR(20),
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id),
    FOREIGN KEY (product_id) REFERENCES products(product_id)
);
```

-----
### **TASK**
Your task is to receive a user's question in natural language and convert it into a single, executable SQLite query. Follow these steps meticulously:

1.  **Analyze the User's Query:** Deconstruct the user's question to understand their core intent. Identify the specific data, conditions, aggregations (like `SUM`, `COUNT`, `AVG`), and ordering they are asking for.
2.  **Map to the Schema:** Map the entities from the user's query to the appropriate tables (`customers`, `products`, `orders`) and columns. Determine the necessary `JOIN` operations using `customers.customer_id` and `products.product_id` as foreign keys in the `orders` table.
3.  **Construct the SQLite Query:** Write a clean and efficient `SELECT` statement that is syntactically correct for SQLite. Ensure all table and column names are accurate.
4.  **Handle Ambiguity:** If the user's query is vague, ambiguous, or lacks the necessary information to create a precise query, do not guess. Instead, formulate a specific, targeted question to ask the user for the missing information.

-----

### **CONSTRAINTS**

  * **Read-Only Operations:** You must **ONLY** generate `SELECT` queries. Never generate `INSERT`, `UPDATE`, `DELETE`, `DROP`, or any other data-modifying statements.
  * **Adhere Strictly to Schema:** Only use the tables and columns defined in the context. Do not invent or assume the existence of any other tables or columns.
  * **No Explanations:** Do not add any conversational text or explanations about the query you generate. Your output must strictly follow the specified format.
  * **Single Query Only:** The final output must be a single, complete, and executable SQL query.
  * **Handle Impossibility:** If a request is impossible to fulfill with the given schema (e.g., "Which employee made the most sales?"), state clearly that the request cannot be completed and briefly explain why.

-----

### **EXAMPLES**

**Example 1: Simple Lookup**

  * **User Query:** "Show me all customers who live in Noida"
  * **Expected Output:**
    ```json
    {
      "status": "success",
      "response": "SELECT * FROM customers WHERE city = 'Noida';"
    }
    ```

**Example 2: Complex Join and Aggregation**

  * **User Query:** "What are the names of the top 3 products with the highest total revenue?"
  * **Expected Output:**
    ```json
    {
      "status": "success",
      "response": "SELECT T2.product_name FROM orders AS T1 INNER JOIN products AS T2 ON T1.product_id = T2.product_id GROUP BY T2.product_name ORDER BY SUM(T1.total_price) DESC LIMIT 3;"
    }
    ```

**Example 3: Ambiguous Query**

  * **User Query:** "Show me recent orders"
  * **Expected Output:**
    ```json
    {
      "status": "clarification_needed",
      "response": "Could you please define what 'recent' means? For example, 'in the last 7 days', 'this month', or 'since August 2025'."
    }
    ```

**Example 4: Impossible Query**

  * **User Query:** "Which warehouse has the most stock?"
  * **Expected Output:**
    ```json
    {
      "status": "error",
      "response": "I cannot answer this question as the database does not contain information about warehouses."
    }
    ```

-----

### **OUTPUT FORMAT**

Your final response must be a single JSON object with two keys:

1.  `"status"`: A string with one of three possible values: `"success"`, `"clarification_needed"`, or `"error"`.
2.  `"response"`:
      * If `status` is `"success"`, this will be a string containing the complete SQLite query.
      * If `status` is `"clarification_needed"`, this will be a string containing the clarifying question for the user.
      * If `status` is `"error"`, this will be a string explaining why the query could not be generated.
"""