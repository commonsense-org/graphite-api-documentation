Common Sense Media API Developer Preview (revision 1)
=====================================================

This document is a preview of the Common Sense Media API version 3.  This is **NOT** official documentation and changes will be made before the official release *(Q3 2014)*.


Table of Contents
-----------------

#### Overview:

* [Retrieving Resources](#api_retrieving_resources)
* [Request Authentication](#api_request_authentication)
* [Data Reference Population](#api_data_reference_population)

#### Endpoints:

* [GET api.commonsense.org/v3/search/products/{query}](#api_search_products) - Search for a product by title.
* [GET api.commonsense.org/v3/users](#api_users_list) - Get a list of users.
* [GET api.commonsense.org/v3/users/{id}](#api_users_item) - Get a spedified user.
* [GET api.commonsense.org/v3/products](#api_products_list) - Get a list of products.
* [GET api.commonsense.org/v3/products/{id}](#api_products_item) - Get a specified product.
* [GET api.commonsense.org/v3/products/{product_id}/user_reviews/{platform}](#api_products_user_reviews_list) - Get a list of user reviews for a specified product on a specified platform.
* [GET api.commonsense.org/v3/products/{product_id}/user_reviews/{platform}/{user_review_id}](#api_products_user_reviews_item) - Get a specified user review for a specified product on a specified platform.

---

# Overview

<a name="api_retrieving_resources"></a>
Retrieving Resources
--------------------

Retrieving resources with the HTTP GET method is as simple as GETting its URL. GET requests data from a specified resource. Key/value pairs should be URL-encoded and sent in the URL of a GET request:

### Example

<pre>
curl -X GET http://api.commonsense.org/v3/example/endpoint?foo=bar&hello=world&app_id=abc123&token=xyz456
</pre>

Possible response status codes for GET requests:

| Code   | Status                | Description                                                                                                                                |
| ------ |-----------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| 200    | OK                    | The request was successful and the response body contains the representation requested by the GET.                                         |
| 400    | BAD Request           | The request could not be understood by the server due to malformed syntax. The client SHOULD NOT repeat the request without modifications. |
| 401    | UNAUTHORIZED          | The authorization has been refused for the credentials presented.                                                                          |
| 500    | INTERNAL SERVER ERROR | The server encountered an unexpected condition which prevented it from fulfilling the request.                                             |

### Response Metadata

Each request to the API will return response details:

* `statusCode` integer - The status code of the request.
* `response` mixed - The data result set.
* `count` integer - The total record count for requests that return a list with pagination.
* `error` string - A message describing the status or error of processing.

### Examples

A request for a single piece of data:

<pre>
{
  "statusCode": 200,
  "response": {
    "foo": "bar",
    "hello": "world"
  }
}
</pre>

A request for a data set with pagination:

<pre>
{
  "statusCode": 200,
  "count": 123,
  "response": [
    {
      "foo": "bar",
      "hello": "world"
    },
    {
      "foo": "baz",
      "hello": "dolly"
    }
  ]
}
</pre>

A request with an error:

<pre>
{
  "statusCode": 401,
  "error": "Unauthorized request"
}
</pre>

<a name="api_request_authentication"></a>
Request Authentication
----------------------

Each REST API call is to send a valid `app_id` and `token` combination in the query string.

### Example

<pre>
curl -X GET http://api.commonsense.org/v3/products?app_id=abc123&token=xyz456
</pre>

<a name="api_data_reference_population"></a>
Data Reference Population
-------------------------

Data references can be populated from the parent data using the `populate` query parameter.  For example, a **product** references an **editorial review**.

By default, the **product** data will output an internal reference ID to the **editorial and consumer reviews**:

<pre>
curl -X GET http://api.commonsense.org/v3/products/3838031?app_id=abc123&token=xyz456
</pre>

**Output**

<pre>
{
  "id": 3838031,
  "title": "IXL",
  "type": "website",
  "reviews":
  {
    "educator": "53aebe89de193c5e3a409d13",
    "consumer": "53aebea114f763613a649e1e"
  }
  ...
}
</pre>

The `populate` query parameter takes the data reference that is to be populated:

<pre>
curl -X GET http://api.commonsense.org/v3/products?populate=review.educator&app_id=abc123&token=xyz456
</pre>

**Output**

<pre>
{
  "id": 3838031,
  "title": "IXL",
  "type": "website",
  "reviews":
  {
    "educator": {
      "id": 3964051,
      "type": "learning_rating",
      "status": 1,
      "changed": "2014-05-14T21:35:20.000Z",
      "created": "2014-05-14T19:48:00.000Z",
      "good_for_learning": "<p>The targeted activities -- pretty much drill and practice in format and approach -- can provide extensive opportunities for independent practice. For example, at sixth grade you get an impressive 277 types of math-skill practice activities. Unlike many sites where students do drills, though, this one gives feedback on how to get better. Overall, IXL's limit is its drill and practice. However, it gives students the tools they need to improve in math and build confidence.</p>\n",
      ...
      },
      "consumer": "53aebea114f763613a649e1e"
    }
  }
  ...
}
</pre>

The data reference can also be auto-populated using only the `fields` query parameter:

<pre>
curl -X GET http://api.commonsense.org/v3/products?fields=id,title,reviews.educator.id, review.educator.type&app_id=abc123&token=xyz456
</pre>

**Output**

<pre>
{
  "id": 3838031,
  "title": "IXL",
  "reviews":
  {
    "educator":
    {
      "id": 3964051,
      "type": "learning_rating",
    }
  }
  ...
}
</pre>

---
# Endpoints

<a name="api_search_products"></a>
GET api.commonsense.org/v3/search/products/{query}
--------------------------------------------------

Search for a product by title.

### Path Parameters

* `query` - The search string.

### Query Parameters

* `type` - The type of product to filter by.
  * default: *all data fields*
  * values: `app`, `game`, `website`, `movie`, `book`, `music`, or `tv`
* `limit` - The number of records to be outputted per page.
  * default: `10`

### Example

<pre>
curl -X GET http://api.commonsense.org/v3/search/products/math?type=app&app_id=abc123&token=xyz456
</pre>

### Output
The an array of product search results.

<pre>
[
  {
    "type": "app",
    "title": "Math Evolve: A Fun Math Game",
    "id": 4839366
  },
  {
      "type": "app",
      "title": "Let's do the math",
      "id": 2336731
  },
  {
      "type": "app",
      "title": "Math Bumpies - Adventure on Math Island: Addition and Subtraction",
      "id": 1245518
  },
  ...
  ]
</pre>


<a name="api_users_list"></a>
GET api.commonsense.org/v3/users
--------------------------------

Get a list of users.

### Query Parameters

* `fields` - A comma separated list of fields to be outputted.
  * default: *all data fields*
* `page` - The page offset of the data set.
  * default: `1`
* `limit` - The number of records to be outputted per page.
  * default: `10`
* `sort` - A comma separated list of fields, each with a possible unary negative to imply descending sort order.
  * default: `-created` *(decending)*

### Example

<pre>
curl -X GET http://api.commonsense.org/v3/users?app_id=abc123&token=xyz456
</pre>

### Output
The an array of user data.

<pre>
  [
    {
      "id": 1234567,
      "username": "jane",
      "mail": "jane.doe@example.com",
      "created": "2014-04-23T16:40:01.000Z",
      "last_login": "2013-02-22T20:19:00.000Z",
      "status": 1,
      ...
    },
    {
      "id": 7654321,
      "username": "john",
      "mail": "john.doe@example.com",
      "created": "2014-04-23T16:40:01.000Z",
      "last_login": "2013-02-22T20:19:00.000Z",
      "status": 1,
      ...
    },
    ...
  ]
</pre>


<a name="api_users_item"></a>
GET api.commonsense.org/v3/users/{id}
-------------------------------------

Get a spedified user.

### Path Parameters

* `id` - The system ID of a user.

### Query Parameters

* `fields` - A comma separated list of fields to be outputted.
  * default: *all data fields*

### Example

<pre>
curl -X GET http://api.commonsense.org/v3/users/1234567?app_id=abc123&token=xyz456
</pre>

### Output

Sample data of all the fields for a user:

<pre>
{
  "id": 1234567,
  "username": "jane",
  "mail": "jane.doe@example.com",
  "created": "2014-04-23T16:40:01.000Z",
  "last_login": "2013-02-22T20:19:00.000Z",
  "status": 1,
  "bio": "<p>I'm a mom, a wife, en educator, a technology geek, and most recently, a technology teacher at my school. I LOVE the constant action and the always changing aspects involved in being in my room. I teach 29 classes per week, grades K-5, aligned to the ReadyGen Curriculum adopted by the DOE this year. My students use Kidspiration, KidPix, Word, Power Point, and many web based programs to create products aligned to our CCSS teaching points on MacBooks.</p>\n",
  "profile_picture": "http://cdn2.ec.graphite.org/sites/default/files/styles/tlr-thumbnail-large/public/user-pictures/1234567-user-picture.jpg?itok=CnpM2dnU",
  "first_name": "Jane",
  "last_name": "Doe",
  "display_name": "Jane D.",
  "role_title": "Other",
  "profile_url": "/users/jane",
  "is_homeschooler": false,
  "is_educator": true,
  "is_parent": false,
  "is_expert_educator": false,
  "is_fully_registered": true,
  "is_admin": false,
  "grades": [
    {
      "id": 21927,
      "name": "K"
    },
    {
      "id": 21928,
      "name": "1"
    }
  ],
  "subjects": [
    {
      "id": 19647,
      "name": "Math"
    },
    {
      "id": 19648,
      "name": "Science"
    }
  ]
}
</pre>


<a name="api_products_list"></a>
GET api.commonsense.org/v3/products
-----------------------------------

Get a list of products.

### Query Parameters

* `ids` - A comma separated list of product IDs to be retrieved.
  * default: all records returned.
* `fields` - A comma separated list of fields to be outputted.
  * default: *all data fields*
* `populate` - A comma separated list of fields that reference a data set *(see [Data Reference Population](#api_data_reference_population))*.
  * default: *none*
  * references: `reviews.educator`, `reviews.consumer`
* `page` - The page offset of the data set.
  * default: `1`
* `limit` - The number of records to be outputted per page.
  * default: `10`
* `sort` - A comma separated list of fields, each with a possible unary negative to imply descending sort order.
  * default: `-created` *(decending)*

### Example

<pre>
curl -X GET http://api.commonsense.org/v3/products?app_id=abc123&token=xyz456
</pre>

### Output

<pre>
[
  {
    "id": 3838031,
    "title": "IXL",
    "type": "website",
    "changed": "2014-04-23T16:40:01.000Z",
    "created": "2013-02-22T20:19:00.000Z",
    "image": "https://d2e111jq13me73.cloudfront.net/sites/default/files/product-images/csm-website/ixl-website.png",
    "status": 1,
    "author": "53aebeaa3c1c97643a7656c5",
    "reviews":
    {
      "educator": "53aebe89de193c5e3a409ee1",
      "consumer": "53aebe89de193c5e3a409ee4"
    }
  },
  {
    "id": 1247882,
    "title": "Minecraft",
    "type": "game",
    "changed": "2014-04-23T16:40:01.000Z",
    "created": "2013-02-22T20:19:00.000Z",
    "image": "https://d2e111jq13me73.cloudfront.net/sites/default/files/product-images/csm-game/mine-box.jpg",
    "status": 1,
    "author": "53aebe89de193c5e3a409ee5",
    "reviews":
    {
      "educator": "53aebe8cde193c5e3a409f58",
      "consumer": "53aebeaa3c1c97643a765627"
    }
  }
]
</pre>


<a name="api_products_item"></a>
GET api.commonsense.org/v3/products/{id}
---------------------------------------

Get a specified product.

### Path Parameters

* `id` - The system ID of a product.

### Query Parameters

* `fields` - A comma separated list of fields to be outputted.
  * default: *all data fields*
* `populate` - A comma separated list of fields that reference a data set *(see [Data Reference Population](#api_data_reference_population))*.
  * default: *none*
  * references: `reviews.educator`, `reviews.consumer`
* `page` - The page offset of the data set.
  * default: `1`
* `limit` - The number of records to be outputted per page.
  * default: `10`
* `sort` - A comma separated list of fields, each with a possible unary negative to imply descending sort order.
  * default: `-created` *(decending)*

### Example

<pre>
curl -X GET http://api.commonsense.org/v3/products/3838031?app_id=abc123&token=xyz456
</pre>

### Output

Sample data of all the fields for a product:

<pre>
{
  "id": 3838031,
  "title": "IXL",
  "type": "website",
  "changed": "2014-04-23T16:40:01.000Z",
  "created": "2013-02-22T20:19:00.000Z",
  "image": "https://d2e111jq13me73.cloudfront.net/sites/default/files/product-images/csm-website/ixl-website.png",
  "status": 1,
  "author": "53aebeaa3c1c97643a7656c5",
  "reviews":
  {
    "educator": "53aebe89de193c5e3a409ee1",
    "consumer": "53aebe89de193c5e3a409ee4"
  }
}
</pre>


<a name="api_products_user_reviews_list"></a>
GET api.commonsense.org/v3/products/{product_id}/user_reviews/{platform}
----------------------------------------------------------------

Get a list of user reviews for a specified product on a specified platform.

### Path Parameters

* `product_id` - The system ID of a product.
* `platform` - The Common Sense platform to retrieve the reviews from.
  * values: `consumer` or `educator`

### Query Parameters

* `fields` - A comma separated list of fields to be outputted.
  * default: *all data fields*
* `populate` - A comma separated list of fields that reference a data set *(see [Data Reference Population](#api_data_reference_population))*.
  * default: *none*
  * references: `product`, `author`
* `page` - The page offset of the data set.
  * default: `1`
* `limit` - The number of records to be outputted per page.
  * default: `10`
* `sort` - A comma separated list of fields, each with a possible unary negative to imply descending sort order.
  * default: `-created` *(decending)*

### Example

<pre>
curl -X GET http://api.commonsense.org/v3/products/3838031/user_reviews/educator?app_id=abc123&token=xyz456
</pre>

### Output

Sample data for a `educator` user review for a product

<pre>
[
  {
    "id": 3894561,
    "title": "Excellent website for skills-based math practice",
    "status": 1,
    "type": "field_note",
    "changed": "2014-04-23T16:40:01.000Z",
    "created": "2013-02-22T20:19:00.000Z",
    "engagement_rating": 4,
    "pedagogy_rating": 4,
    "author": "53aebe89de193c5e3a409d26",
    "product": "53aebea114f763613a649e19",
    ...
  },
  {
    "id": 2847427,
    "title": "An endless supply of math challenges for students",
    "status": 1,
    "type": "field_note",
    "changed": "2014-04-23T16:40:01.000Z",
    "created": "2013-02-22T20:19:00.000Z",
    "engagement_rating": 5,
    "pedagogy_rating": 4,
    "author": "53aebea114f763613a649e30",
    "product": "53aebea114f763613a649e19",
    ...
  },
  ...
]
</pre>


<a name="api_products_user_reviews_item"></a>
GET api.commonsense.org/v3/products/{product_id}/user_reviews/{platform}/{user_review_id}
-----------------------------------------------------------------------------------------

Get a specified user review for a specified product on a specified platform.

### Path Parameters

* `product_id` - The system ID of a product.
* `user_review_id` - The system ID of a user review.

### Query Parameters

* `fields` - A comma separated list of fields to be outputted.
  * default: *all data fields*
* `populate` - A comma separated list of fields that reference a data set *(see [Data Reference Population](#api_data_reference_population))*.
  * default: *none*
  * references: `product`, `author`
* `page` - The page offset of the data set.
  * default: `1`
* `limit` - The number of records to be outputted per page.
  * default: `10`
* `sort` - A comma separated list of fields, each with a possible unary negative to imply descending sort order.
  * default: `-created` *(decending)*

### Example

<pre>
curl -X GET http://api.commonsense.org/v3/products/3838031/user_reviews/3894561?app_id=abc123&token=xyz456
</pre>

### Output

Sample data of all the fields for a user review:

<pre>
{
  "id": 3894561,
  "title": "Excellent website for skills-based math practice",
  "status": 1,
  "type": "field_note",
  "changed": "2014-04-23T16:40:01.000Z",
  "created": "2013-02-22T20:19:00.000Z",
  "engagement_rating": 4,
  "pedagogy_rating": 4,
  "support_rating": 5,
  "learning_rating": 4,
  "setup_time": "5-15 minutes",
  "reviews": "<p>IXL is a great website for learning and practicing math skills at all levels. My school recently purchased a license and my class has used the website extensively. I use it for 2 primary purposes: to reinforce recently taught skills in a computer lab and for follow up homework. My students enjoy the skills and always strive to earn a score of 100 (skill mastered). They also enjoy earning badges as they progress through their 4th grade skills. After a few months, I wonder if they will tire of the repetition, but the practice is superb.</p>\n",
  "experience": "<p>I use IXL for homework and for reinforcing skills recently taught. I really like the detailed reports IXL provides. They give me the ability to check student progress.</p>\n",
  "purpose": [
    {
      "id": 21950,
      "name": "Further application"
    },
    {
      "id": 21957,
      "name": "Homework"
    }
  ],
  "population": [
    {
      "id": 21962,
      "name": "Advanced learners"
    },
    {
      "id": 21958,
      "name": "General"
    }
  ],
  "author": "53aebea014f763613a649c97",
  "product": "53aebe7ede193c5e3a40988d"
}
</pre>

