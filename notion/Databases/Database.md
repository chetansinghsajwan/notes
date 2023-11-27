**Database:** Database is collection of database pages.
**Database Page:** A normal page with additional metadata(properties).
**Database Source:** A source is what contains the database internally, it’s not accessible by the user, only referenced through **Database**.
**Database View:** A view defines how the database is viewed by the user, like, table, list or board.
### Difference between Database and Database Source
Database source is the entity that actually contains the actual data, and cannot be accessed by the user. It’s internally managed by notion. Database is the entity that provides user the API to access and reference the source. It also acts as a holder for database views. A database that doesn’t contain its own source and references source of other database, is called linked database.
### Full Page Database vs Inline Database
The difference between **Full Page Database** and **Inline Database** is how they are accessed by the user.
- **Full Page Database** is accessed by opening it in its own full page, thus the name **Full Page Database**.
- **Inline Database** is expanded right where it is stored, providing an inline view.
- **Inline Database** can be expanded, and becomes **Full Page Database**.
- They both can be converted into each other, after all it’s just the presentation.
- There is no difference in how the data is stored or the functionality offered.
  

> [!important]  
> Note: Users with Can edit content access will still be able to create linked databases and edit views, sorts, and filters in that linked database. Learn more about linked databases →  
## Lock Views
# References

> [!info] Intro to databases – Notion Help Center  
> Databases in Notion are collections of pages.  
> [https://www.notion.so/help/intro-to-databases](https://www.notion.so/help/intro-to-databases)