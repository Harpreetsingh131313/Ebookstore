
# E-Bookstore Database Design

## Duties Assigned
- **Design and Documentation**: [Pooja Talaniya]
- **Database Schema**: [Harpreet Singh]
- **SQL Queries**: [Harpreet singh]
- **TypeScript Interface**: [Pawanjot Kaur]

## Table Definitions

### 1. Books Table
| Column Name     | Data Type        | Description                        |
|-----------------|------------------|------------------------------------|
| Book_name       | SERIAL           | Primary Key                        |
| Title           | VARCHAR(255)     | Title of the book                  |
| Genre           | VARCHAR(100)     | Genre of the book                  |
| Writer_Id       | VARCHAR(30)      | Foreign Key citing Writers         |
| Publisher_Id    | INT              | Foreign Key citing Publishers      |
| Published_Date  | DATE             | Date when the book was published   |
| Book_Type       | VARCHAR(40)      | Physical, e-book, or audiobook     |
| Price           | NUMERIC(10, 2)   | Price of the book                  |
| Rating          | NUMERIC(5, 3)    | Rating of the user                 |

### 2. Buyers Table
| Column Name     | Data Type        | Description                        |
|-----------------|------------------|------------------------------------|
| Buyer_name      | SERIAL           | Primary Key                        |
| Name            | VARCHAR(30)      | Name of the Buyer                  |
| Email           | VARCHAR(50)      | Unique email of the customer       |
| Joining_Date    | DATE             | Date when the customer joined      |

### 3. Purchases Table
| Column Name     | Data Type        | Description                        |
|-----------------|------------------|------------------------------------|
| Purchase_Id     | SERIAL           | Primary Key                        |
| Buyer_name      | VARCHAR(30)      | Foreign Key Citing Customers       |
| Book_name       | VARCHAR(30)      | Foreign Key Citing Books           |
| Purchase_Date   | DATE             | Date of purchase                   |
| Quantity        | INT              | Number of books purchased          |
| Total_Price     | NUMERIC(10, 2)   | Total price of the purchase        |

### 4. Feedbacks Table
| Column Name     | Data Type        | Description                        |
|-----------------|------------------|------------------------------------|
| Feedback_Id     | SERIAL           | Primary Key                        |
| Buyer_name      | VARCHAR(30)      | Foreign Key Citing Buyer           |
| Book_name       | VARCHAR(30)      | Foreign Key Citing Books           |
| Feedback_Date   | DATE             | Date of the Feedback               |
| Rating          | NUMERIC(5, 3)    | Rating given by the Buyer          |
| Comment         | TEXT             | Feedback comment                   |

### 5. Writers Table
| Column Name     | Data Type        | Description                        |
|-----------------|------------------|------------------------------------|
| Writer_Id       | SERIAL           | Primary Key                        |
| Name            | VARCHAR(30)      | Name of the writer                 |
| Bio             | Text             | Biography of the writer            |

### 6. Publishers Table
| Column Name     | Data Type        | Description                        |
|-----------------|------------------|------------------------------------|
| Publisher_Id    | SERIAL           | Primary Key                        |
| Name            | VARCHAR(30)      | Name of the publisher              |
| Address         | VARCHAR(200)     | Address of the Publisher           |



## SQL Code for Creating Tables

```sql
CREATE TABLE Books (
    Book_name SERIAL PRIMARY KEY,
    Title VARCHAR(255) NOT NULL,
    Genre VARCHAR(100) NOT NULL,
    Writer_Id VARCHAR(30) NOT NULL,
    Publisher_Id INT NOT NULL,
    Published_Date DATE NOT NULL,
    Book_Type VARCHAR(40) NOT NULL,
    Price NUMERIC(10, 2) NOT NULL,
    Rating NUMERIC(5, 3)
);

CREATE TABLE Buyers (
    Buyer_name SERIAL PRIMARY KEY,
    Name VARCHAR(30) NOT NULL,
    Email VARCHAR(50) UNIQUE NOT NULL,
    Joining_Date DATE NOT NULL
);

CREATE TABLE Purchases (
    Purchase_Id SERIAL PRIMARY KEY,
    Buyer_name INT NOT NULL,
    Book_name INT NOT NULL,
    Purchase_Date DATE NOT NULL,
    Quantity INT NOT NULL,
    Total_Price NUMERIC(10, 2) NOT NULL,
    FOREIGN KEY (Buyer_name) REFERENCES Buyers(Buyer_name),
    FOREIGN KEY (Book_name) REFERENCES Books(Book_name)
);

CREATE TABLE Feedbacks (
    Feedback_Id SERIAL PRIMARY KEY,
    Buyer_name INT NOT NULL,
    Book_name INT NOT NULL,
    Feedback_Date DATE NOT NULL,
    Rating NUMERIC(5, 3) NOT NULL,
    Comment TEXT,
    FOREIGN KEY (Buyer_name) REFERENCES Buyers(Buyer_name),
    FOREIGN KEY (Book_name) REFERENCES Books(Book_name)
);

CREATE TABLE Writers (
    Writer_name SERIAL PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Bio TEXT
);

CREATE TABLE Publishers (
    Publisher_Id SERIAL PRIMARY KEY,
    Name VARCHAR(30) NOT NULL,
    Address VARCHAR(200)
);

-- DDL for Buyers Table
CREATE TABLE Buyers (
    Buyer_name SERIAL PRIMARY KEY,
    Name VARCHAR(30) NOT NULL,
    Email VARCHAR(50) UNIQUE NOT NULL,
    Joining_Date DATE NOT NULL
);

-- DML for Buyers Table
INSERT INTO Buyers (Name, Email, Joining_Date) VALUES
('Harpreet Singh', 'harshsingh9676@gmail.com', '2024-04-18'),
('Pooja Talaniya', 'poojaTalaniya@gmail.com', '2024-04-10'),
('Pawanjot kaur', 'pawanjotkaur@gmail.com', '2024-05-05');

## CRUD COMMANDS

-- Create
INSERT INTO Buyers (Name, Email, Joining_Date) VALUES ('Hora Saab', 'horasaab13@gmail.com', '2024-06-28');

-- Read
SELECT * FROM Buyers WHERE Buyer_name = 2;

-- Update
UPDATE Buyers SET Email = 'balbirkaur13@gmail.com' WHERE Buyer_name = 2;

-- Delete
DELETE FROM Buyers WHERE Buyer_name = 2;

##Power writers 
SELECT Writer_Id, COUNT(*) AS Book_Count
FROM Books
WHERE Genre = 'Specific Genre' AND Published_Date > NOW() - INTERVAL '10 years'
GROUP BY Writer_Id
HAVING COUNT(*) > 10;

##LOYAL CONSUMERS WHO SPENT X DOLLARS IN PAST YEARS
SELECT Buyer_name, SUM(Total_Price) AS Total_Spent
FROM Purchases
WHERE Purchase_Date > NOW() - INTERVAL '10 years'
GROUP BY Buyer_name
HAVING SUM(Total_Price) > 10;

-- Inserting 10 entries into Feedbacks table

INSERT INTO Feedbacks (Buyer_Id, Book_Id, Feedback_Date, Rating, Comment)
VALUES
(1, 1, '2024-06-30', 5.0, 'Excellent read, adore the characters!'),
(2, 3, '2024-06-29', 3.8, 'The storyline had unexpected twists, although the pace was a little sluggish.'),
(3, 2, '2024-06-28', 2.2, 'Couldn't stop reading, really enjoyed & would recommend!'),
(1, 4, '2024-06-27', 2.0, 'Amazing narrative, captivated me until the conclusion.'),
(2, 5, '2024-06-26', 5.0, 'Enjoyable read, but I was anticipating more plot twists.'),
(3, 1, '2024-06-25', 3.7, 'One of the best books I have read in this year'),
(1, 3, '2024-06-24', 2.3, 'Engaging and well-written by the Author.'),
(2, 4, '2024-06-23', 2.9, 'Appreciated the growth of the characters.'),
(3, 5, '2024-06-10', 4.5, 'Decent book, just not in my preferred genre.'),
(1, 2, '2024-06-11', 5.0, 'Sci-Fi & Action storyline, ideal for fans of suspenseful movies.');

-- Verify by selecting the 10 most recent feedbacks
SELECT f.Feedback_Id, f.Comment, f.Feedback_Date, b.Name AS Buyer_Name, bk.Title AS Book_Title
FROM Feedbacks f
JOIN Buyers b ON f.Buyer_Id = b.Buyer_Id
JOIN Books bk ON f.Book_Id = bk.Book_Id
ORDER BY f.Feedback_Date DESC
LIMIT 10;

##WELL REVIEWED BOOKS
SELECT Book_name, Title, Rating
FROM Books
WHERE Rating > (SELECT AVG(Rating) FROM Books);

##THE MOST FAMOUS GENRE BY SALES
SELECT Genre, SUM(Quantity) AS Total_Sales
FROM Books b
JOIN Purchases p ON b.Book_name = p.Book_name
GROUP BY Genre
ORDER BY Total_Sales DESC
LIMIT 1;

##THE 10 MOST RECENT FEEDBACKS POSTED BY BUYERS
SELECT r.Feedback_Id, r.Comment, r.Feedback_Date, c.Name, b.Title
FROM Feedbacks r
JOIN Buyers c ON r.Buyer_name = c.Buyer_name
JOIN Books b ON r.Book_name = b.Book_name
ORDER BY r.Feedback_Date DESC
LIMIT 10;

interface Buyer {
  Buyer_name: number;
  Name: string;
  Email: string;
  Joining_Date: Date;
}

interface BuyerService {
  // Create a new Buyer
  createBuyer(buyer: Omit<Buyer, 'Buyer_name'>): Promise<Buyer>;

  // Retrieve a Buyer by Buyer_name
  getBuyerById(buyerName: number): Promise<Buyer | null>;

  // Update a Buyer's details
  updateBuyer(buyer: Buyer): Promise<Buyer>;

  // Delete a Buyer by Buyer_name
  deleteBuyer(buyerName: number): Promise<void>;

  // Retrieve all Buyers
  getAllBuyers(): Promise<Buyer[]>;
}

class BuyerServiceImpl implements BuyerService {
  private buyers: Buyer[] = [];

  async createBuyer(buyer: Omit<Buyer, 'Buyer_name'>): Promise<Buyer> {
    const newBuyer: Buyer = { Buyer_name: this.buyers.length + 1, ...buyer };
    this.buyers.push(newBuyer);
    return newBuyer;
  }

  async getBuyerById(buyerName: number): Promise<Buyer | null> {
    const buyer = this.buyers.find(b => b.Buyer_name === buyerName);
    return buyer ? { ...buyer } : null;
  }

  async updateBuyer(buyer: Buyer): Promise<Buyer> {
    const index = this.buyers.findIndex(b => b.Buyer_name === buyer.Buyer_name);
    if (index !== -1) {
      this.buyers[index] = buyer;
      return { ...buyer };
    } else {
      throw new Error('Buyer not found');
    }
  }

  async deleteBuyer(buyerName: number): Promise<void> {
    const index = this.buyers.findIndex(b => b.Buyer_name === buyerName);
    if (index !== -1) {
      this.buyers.splice(index, 1);
    } else {
      throw new Error('Buyer not found');
    }
  }

  async getAllBuyers(): Promise<Buyer[]> {
    return [...this.buyers];
  }
}

// Usage of the BuyerService
(async () => {
  const buyerService = new BuyerServiceImpl();

  // Create new buyers
  await buyerService.createBuyer({ Name: 'Harpreet Singh', Email: 'harshsingh9676@gmail.com', Joining_Date: new Date('2024-04-18') });
  await buyerService.createBuyer({ Name: 'Pooja Talaniya', Email: 'poojaTalaniya@gmail.com', Joining_Date: new Date('2024-04-10') });
  await buyerService.createBuyer({ Name: 'Pawanjot Kaur', Email: 'pawanjotkaur@gmail.com', Joining_Date: new Date('2024-05-05') });

  // Read a buyer by ID
  const buyer = await buyerService.getBuyerById(2);
  console.log('Buyer with ID 2:', buyer);

  // Update a buyer's email
  if (buyer) {
    buyer.Email = 'balbirkaur13@gmail.com';
    const updatedBuyer = await buyerService.updateBuyer(buyer);
    console.log('Updated Buyer:', updatedBuyer);
  }

  // Delete a buyer by ID
  await buyerService.deleteBuyer(2);
  console.log('Deleted Buyer with ID 2');

  // Get all buyers
  const allBuyers = await buyerService.getAllBuyers();
  console.log('All Buyers:', allBuyers);
})();



