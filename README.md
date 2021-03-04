## Description
A very common problem in enterprise software development is related to handling postal addresses. Nearly every country has a particular, and peculiar, way to structure a postal address. [This](http://www.bitboost.com/ref/international-address-formats.html#Formats) is a high-level overview of the problem complete with details for a number of countries.

We have attempted to address (no pun intended) this problem by delivering the following:

- A web-based user interface (a form) that can capture a country-specific address format entered by an end user
   - The form must dynamically adjust to capture the address
   - Validation of the address formats and related data must occur in a reasonable time for the user
   - The end user should be able to select data from appropriate user interface elements and not just type everything in free-form
   - Where possible, default values and constrained lists should be presented to the user
- A way for a user to search for a given address based on the country-specific format
- A way for a user to search across countries to find "matching" addresses and display them in the application
- An API callable via HTTP (curl or postman) because we might want to sell access to this application
   - Documentation for the API, preferably available alongside the API itself (think swagger / OpenAPI)

## Run
`~/SWArch/5200_flask_app: flask run`

## Docs
[address sheet](https://docs.google.com/spreadsheets/d/1xICn3orrbPI6uKnEBG2G12yB0st0GDQ7WzzVTKFiuEw/edit#gid=0)

[Detailed Design Document](https://docs.google.com/document/d/1Y2ppWUZipnUZcrbwpIJknaRZC5otsBlZKqsKTe8RTR0/edit)

[mongoDB](https://cloud.mongodb.com/v2/5e489c3e79358e377c805caa#clusters)

## UI
* The `UI` relies on the `Bootstrap` and `JavaScript`.
* Navigating to the `upload` page, you will be greeted with a drop down menu where you choose the country to upload.
* Selecting a country triggers an `Ajax` request to a `form factory` that returns correct version of the form based on country selected.
* Client side validation is responsible for ensuring that the `required fields` for that country are not empty.

### Form Factory
* It is instantiated through the factory pattern.
* It consults rules tables for the fields to be provided for countries.

## API
* The `API` can be invoked by the application as well as the user.
* The `API` receives requests, either from the UI or from a service such as `curl`.
* When calling the `API` for upload or query, `Country Data` must always be provided in `JSON` format.
* `API` provides `four` endpoints.
    * The `home(/api)` endpoint gives information about accessible endpoints and accepted data format.
    * The `upload(/api/upload)` and `query(/api/query)` endpoints are invoked by the UI through `POST` requests to upload and query the address data. 
    * The address can also be directly updated using `/api/update/{addressID}` endpont. This accepts the `PUT` requests and expects `JSON` payload with the addressID.  

## Database Ops
1. start mongodb: `mongo --authenticationDatabase admin`
2. use mongodb terminal: `mongo --port 27017 --authenticationDatabase admin`
3. create a db called `arch` in the terminal: `use arch;` //it's not created actually until you insert some ducuments into it
4. create a collection: `db.createCollection("addresses");`
4. insert a document into the collection: `db.addresses.insert({"name":"test"});`

## Helpful links

[MongoDB tutorial](https://www.tutorialspoint.com/mongodb/)
