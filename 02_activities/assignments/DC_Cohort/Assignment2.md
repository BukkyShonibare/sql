# Assignment 2: Design a Logical Model and Advanced SQL

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

#### Submission Parameters:
* Submission Due Date: `November 12, 2025`
* Weight: 70% of total grade
* The branch name for your repo should be: `assignment-two`
* What to submit for this assignment:
    * This markdown (Assignment2.md) with written responses in Section 1 and 4
    * Two Entity-Relationship Diagrams (preferably in a pdf, jpeg, png format).
    * One .sql file 
* What the pull request link should look like for this assignment: `https://github.com/<your_github_username>/sql/pulls/<pr_id>`
    * Open a private window in your browser. Copy and paste the link to your pull request into the address bar. Make sure you can see your pull request properly. This helps the technical facilitator and learning support staff review your submission easily.

Checklist:
- [ ] Create a branch called `assignment-two`.
- [ ] Ensure that the repository is public.
- [ ] Review [the PR description guidelines](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#guidelines-for-pull-request-descriptions) and adhere to them.
- [ ] Verify that the link is accessible in a private browser window.

If you encounter any difficulties or have questions, please don't hesitate to reach out to our team via our Slack. Our Technical Facilitators and Learning Support staff are here to help you navigate any challenges.

***

## Section 1:
You can start this section following *session 1*, but you may want to wait until you feel comfortable wtih basic SQL query writing. 

Steps to complete this part of the assignment:
- Design a logical data model
- Duplicate the logical data model and add another table to it following the instructions
- Write, within this markdown file, an answer to Prompt 3


###  Design a Logical Model

#### Prompt 1
Design a logical model for a small bookstore. 📚

At the minimum it should have employee, order, sales, customer, and book entities (tables). Determine sensible column and table design based on what you know about these concepts. Keep it simple, but work out sensible relationships to keep tables reasonably sized. 

Additionally, include a date table. 

There are several tools online you can use, I'd recommend [Draw.io](https://www.drawio.com/) or [LucidChart](https://www.lucidchart.com/pages/).

**HINT:** You do not need to create any data for this prompt. This is a conceptual model only. 

#### Prompt 2
We want to create employee shifts, splitting up the day into morning and evening. Add this to the ERD.

#### Prompt 3
The store wants to keep customer addresses. Propose two architectures for the CUSTOMER_ADDRESS table, one that will retain changes, and another that will overwrite. Which is type 1, which is type 2? 

**HINT:** search type 1 vs type 2 slowly changing dimensions. 

```
There are two main ways to design the customer_address table - one that overwrites the old address and the other that keeps changes in the address.

The first architecture is Type 1, which overwrites changes. When a customer updates their address, the new address simply replaces the old one in the same record. This keeps the data clean and simple, but all history is lost, as the system will only ever show the current address.

The second architecture is Type 2, which retains changes. For this, each time a customer changes their address, a new record is added instead of replacing the old one. This approach will include start and end dates or a sign to show which address is current. This preserves the customer's full address history and allows the system to track where a customer has lived over time.

Essentially, Type 1 overwrites and is simpler, while Type 2 retains history and is better for analysis or tracking changes.

```

***

## Section 2:
You can start this section following *session 4*.

Steps to complete this part of the assignment:
- Open the assignment2.sql file in DB Browser for SQLite:
	- from [Github](./02_activities/assignments/assignment2.sql)
	- or, from your local forked repository  
- Complete each question


### Write SQL

#### COALESCE
1. Our favourite manager wants a detailed long list of products, but is afraid of tables! We tell them, no problem! We can produce a list with all of the appropriate details. 

Using the following syntax you create our super cool and not at all needy manager a list:
```
SELECT 
product_name || ', ' || product_size|| ' (' || product_qty_type || ')'
FROM product
```

But wait! The product table has some bad data (a few NULL values). 
Find the NULLs and then using COALESCE, replace the NULL with a blank for the first column with nulls, and 'unit' for the second column with nulls. 

**HINT**: keep the syntax the same, but edited the correct components with the string. The `||` values concatenate the columns into strings. Edit the appropriate columns -- you're making two edits -- and the NULL rows will be fixed. All the other rows will remain the same.

<div align="center">-</div>

#### Windowed Functions
1. Write a query that selects from the customer_purchases table and numbers each customer’s visits to the farmer’s market (labeling each market date with a different number). Each customer’s first visit is labeled 1, second visit is labeled 2, etc. 

You can either display all rows in the customer_purchases table, with the counter changing on each new market date for each customer, or select only the unique market dates per customer (without purchase details) and number those visits. 

**HINT**: One of these approaches uses ROW_NUMBER() and one uses DENSE_RANK().

2. Reverse the numbering of the query from a part so each customer’s most recent visit is labeled 1, then write another query that uses this one as a subquery (or temp table) and filters the results to only the customer’s most recent visit.

3. Using a COUNT() window function, include a value along with each row of the customer_purchases table that indicates how many different times that customer has purchased that product_id.

<div align="center">-</div>

#### String manipulations
1. Some product names in the product table have descriptions like "Jar" or "Organic". These are separated from the product name with a hyphen. Create a column using SUBSTR (and a couple of other commands) that captures these, but is otherwise NULL. Remove any trailing or leading whitespaces. Don't just use a case statement for each product! 

| product_name               | description |
|----------------------------|-------------|
| Habanero Peppers - Organic | Organic     |

**HINT**: you might need to use INSTR(product_name,'-') to find the hyphens. INSTR will help split the column. 

2. Filter the query to show any product_size value that contain a number with REGEXP. 

<div align="center">-</div>

#### UNION
1. Using a UNION, write a query that displays the market dates with the highest and lowest total sales.

**HINT**: There are a possibly a few ways to do this query, but if you're struggling, try the following: 1) Create a CTE/Temp Table to find sales values grouped dates; 2) Create another CTE/Temp table with a rank windowed function on the previous query to create "best day" and "worst day"; 3) Query the second temp table twice, once for the best day, once for the worst day, with a UNION binding them. 

***

## Section 3:
You can start this section following *session 5*.

Steps to complete this part of the assignment:
- Open the assignment2.sql file in DB Browser for SQLite:
	- from [Github](./02_activities/assignments/assignment2.sql)
	- or, from your local forked repository  
- Complete each question

### Write SQL

#### Cross Join
1. Suppose every vendor in the `vendor_inventory` table had 5 of each of their products to sell to **every** customer on record. How much money would each vendor make per product? Show this by vendor_name and product name, rather than using the IDs.

**HINT**: Be sure you select only relevant columns and rows. Remember, CROSS JOIN will explode your table rows, so CROSS JOIN should likely be a subquery. Think a bit about the row counts: how many distinct vendors, product names are there (x)? How many customers are there (y). Before your final group by you should have the product of those two queries (x\*y). 

<div align="center">-</div>

#### INSERT
1. Create a new table "product_units". This table will contain only products where the `product_qty_type = 'unit'`. It should use all of the columns from the product table, as well as a new column for the `CURRENT_TIMESTAMP`.  Name the timestamp column `snapshot_timestamp`.

2. Using `INSERT`, add a new row to the product_unit table (with an updated timestamp). This can be any product you desire (e.g. add another record for Apple Pie). 

<div align="center">-</div>

#### DELETE 
1. Delete the older record for the whatever product you added.

**HINT**: If you don't specify a WHERE clause, [you are going to have a bad time](https://imgflip.com/i/8iq872).

<div align="center">-</div>

#### UPDATE
1. We want to add the current_quantity to the product_units table. First, add a new column, `current_quantity` to the table using the following syntax.
```
ALTER TABLE product_units
ADD current_quantity INT;
```

Then, using `UPDATE`, change the current_quantity equal to the **last** `quantity` value from the vendor_inventory details. 

**HINT**: This one is pretty hard. First, determine how to get the "last" quantity per product. Second, coalesce null values to 0 (if you don't have null values, figure out how to rearrange your query so you do.) Third, `SET current_quantity = (...your select statement...)`, remembering that WHERE can only accommodate one column. Finally, make sure you have a WHERE statement to update the right row, you'll need to use `product_units.product_id` to refer to the correct row within the product_units table. When you have all of these components, you can run the update statement.
*** 

## Section 4:
You can start this section anytime.

Steps to complete this part of the assignment:
- Read the article
- Write, within this markdown file, <1000 words.

### Ethics

Read: Boykis, V. (2019, October 16). _Neural nets are just people all the way down._ Normcore Tech. <br>
    https://vicki.substack.com/p/neural-nets-are-just-people-all-the

**What are the ethical issues important to this story?**

Consider, for example, concepts of labour, bias, LLM proliferation, moderating content, intersection of technology and society, ect. 


```
Vicki Boykis’ article emphasizes that what appears to be automated intelligence – that is, neural networks and machine learning models – is, at its core, deeply human-driven. This perspective brings several ethical issues to the fore, spanning labour practices, bias, societal impacts of algorithmic systems, and responsibilities in technology deployment.

One of the most striking ethical dimensions is the reliance on human labour for what is often perceived as “automated” work. Boykis traces the history of ImageNet, demonstrating that millions of images were manually labeled by human workers, many via Amazon Mechanical Turk, earning only cents per task. This highlights a persistent ethical issue in AI: the invisibility and undervaluation of labour. The people who perform the tedious, repetitive work of tagging data are essential to the functioning of neural networks, yet they are largely unacknowledged and poorly compensated. This raises concerns about exploitation, informed consent, and fair remuneration in crowdsourced AI labour. Additionally, the metaphor of sewing illustrates a broader point: just as human intuition and dexterity are essential to garment production, so too are human insights central to AI systems. Treating labour as replaceable by “machines” can obscure the real ethical responsibility owed to those workers.

Another major ethical issue stems from the human-derived nature of datasets. Every labeled image in ImageNet or every word in WordNet is subject to human judgment, which inevitably reflects cultural, social, and cognitive biases. Boykis notes instances such as ImageNet Roulette, where facial recognition systems mislabelled people in offensive or inaccurate ways. These errors are not technical glitches alone; they encode systemic social biases into AI. The propagation of these biases raises questions about fairness, justice, and accountability, especially when AI is deployed in sensitive contexts like policing, hiring, or surveillance.

The article also highlights that datasets are inherently political, even at the level of taxonomy creation. Decisions about classification – that is, what counts as a “pizza” or a “hot dog,” which words are synonyms, or which categories exist in a dataset – reflect human assumptions and hierarchies. These choices have ethical consequences because algorithms trained on biased or incomplete datasets can perpetuate inequality at scale.

Boykis’ narrative underscores the opacity of machine learning systems. Most users assume neural networks are self-contained, automated processes, yet they rely on layers of human decision-making stretching back decades – from the creation of the Brown Corpus to the labeling of millions of images in ImageNet. This hidden human labour makes it difficult to trace responsibility for errors or harmful outputs. Ethical AI requires transparency in how training data is constructed, who labels it, and what assumptions are encoded in datasets. Without this, accountability is diffused, making it challenging to address harm caused by misclassifications or offensive outputs.

The article demonstrates the deep societal implications of AI and machine learning. Systems like ImageNet or facial recognition do not exist in a vacuum; they interact with social norms, legal systems, and cultural hierarchies. Mislabeling, offensive categorization, and biased predictions can reinforce stereotypes and marginalize already vulnerable groups. This intersection of technology and society raises ethical questions about the responsibilities of AI researchers and companies to anticipate and mitigate social harm. Boykis notes that Dr. Fei-Fei Li and the ImageNet team are now attempting to correct fairness issues, emphasizing that ethical oversight must be ongoing and proactive.

A related ethical issue is the role of human oversight in moderating AI outputs. While AI systems can scale rapidly, they remain fallible and sensitive to biased or incomplete data. Boykis’ example of mislabelled images highlights the need for continuous human monitoring, auditing, and intervention to prevent offensive or harmful outputs. This has broader implications for the proliferation of large language models (LLMs) and other AI tools: the more widely these systems are deployed, the greater the potential societal impact if human oversight is insufficient.

Finally, the article raises ethical concerns about the proliferation of AI tools without addressing foundational human and societal inputs. As AI becomes more embedded in everyday life, there is a risk of over-reliance on seemingly autonomous systems, obscuring the human labour, bias, and choices embedded in them. The ethical challenge is to balance technological advancement with social responsibility, ensuring that automation does not displace recognition of human contributions, exacerbate inequality, or normalize biased outputs.

Boykis’ article illuminates a fundamental truth: machine learning is not purely automated; it rests on layers of human effort, judgment, and societal context. Ethical considerations arise at every stage – from the labour exploited in dataset creation, to the biases encoded into models, to the societal impacts of misclassifications, and the opacity of AI decision-making. Recognizing these human contributions is essential to developing ethical AI systems. Ethical AI cannot be disentangled from the humans who build, label, and maintain the datasets and algorithms that power it.
```
