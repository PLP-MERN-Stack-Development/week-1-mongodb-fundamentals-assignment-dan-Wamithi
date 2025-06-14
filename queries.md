### Task 1: MongoDB Setup
1. creating database
    -- use plp_bookstore

2. creating collection books 
    -- use books



### Task 2: Basic CRUD Operations
1. find all bookks 
    --db.books.find()

2. Find books published after a certain year
   -- db.books.find({ published_year: { $lte: 1930 } })
   -- db.books.find({published_year: {$gt:1970}})

3. Find books by a specific author
    -- db.books.find({ genre: "Romance" })
    -- db.books.find({genre:"Adenture"})
    -- db.books.find({ genre:"Gothic Fiction"})


4. Update the price of a specific book
    -- db.books.updateOne({title:"Moby Dick"},
     {$set: {price: 16}}
    )
     --db.books.updateOne(
        {author: "Emily Brontë"},
        {$set: {price: 18}}
    )
5. Delete a book by its title
    -- db.books.deleteOne({ title: "Animal Farm" })


### Task 3
1. query to find books that are both in stock and published after 2010
    -- db.books.find(
        {in_stock:true,
        published_year:{$gt: 2010}}
    )
2. projection to return only the title, author, and price fields in your queries
    --db.books.find(
        {in_stock:true},
        {
            title:1,
            author:1,
            price:1
        }
    )
3. sorting to display books by price (both ascending and descending)
    -- db.ooks.find().sort(price: 1) // asc order
    -- db.books.find().sort(price:-1)// dsc order

4.  Use the `limit` and `skip` methods to implement pagination (5 books per pa
    -- db.books.find(
         {},
         { title: 1, author: 1, price: 1, _id: 0 }
         ).sort({ price: 1 }).skip(0).limit(5)
5. 
### Task 4: Aggregation Pipeline
a.  aggregation pipeline to calculate the average price of books by genre
 --db.books.aggregate([
    {
        $group: {
            _id: "$genre",
            averagePrice: { $avg: "$price" }
        }
    },
    {
        $sort: { averagePrice: -1 }
    }
     ])

 b. --db.books.aggregate([
    {  
      $group: {
     _id: "$author",
     book_count: { $sum: 1 }
      }
    },
      { $sort: { book_count: -1 } },
        { $limit: 1 }
    ])



d.  Implement a pipeline that groups books by publication decade and counts them
    -- db.books.aggregate([
        {
           $group: {
            _id: { $subtract: [ "$published_year", { $mod: [ "$published_year", 10 ] } ] },
                  count: { $sum: 1 }
                 }    
         },
            { $sort: { _id: 1 } }
        ])


### Task 5: Indexing 
    -- Create an index on the `title` field for faster searches
    db.books.createIndex({ title: 1 })


    -- db.books.createIndex({ author: 1, published_year: 1 })


    --db.books.getIndexes()
    
    without Index
    -- db.books.find({ title: "1984" }).explain("executionStats")
    With Indexing
    -- db.books.createIndex({ title: 1 })
        db.books.find({ title: "1984" }).explain("executionStats")