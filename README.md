Common Sense Education API
==========================

This document is an overview of the Common Sense Education API version 3.


Table of Contents
-----------------

#### Overview:

* [URL Patterns](#api-url-patterns)
* [Retrieving Resources](#api-retrieving-resources)
* [Request Authentication](#api-request-authentication)

#### Endpoints:

* [GET /v3/education/products](#api-products-list) - Get a list of products with Learning Rating review.
* [GET /v3/education/products/{id}](#api-products-item) - Get a specified product with Learning Rating review.
* [GET /v3/education/flows](#api-flows-list) - Get a list of Lesson Flows.
* [GET /v3/education/flows/{id}](#api-flows-item) - Get a specified Lesson Flow.
* [GET /v3/education/blogs](#api-blogs-list) - Get a list of blog posts.
* [GET /v3/education/blogs/{id}](#api-blogs-item) - Get a specified blog post.
* [GET /v3/education/lists](#api-lists-list) - Get a list of Top Picks lists.
* [GET /v3/education/lists/{id}](#api-lists-item) - Get a specified Top Picks List.
* [GET /v3/education/boards](#api-boards-list) - Get a list of Boards.
* [GET /v3/education/boards/{id}](#api-boards-item) - Get a specified Board.
* [GET /v3/education/user_reviews](#api-user-reviews-list) - Get a list of product Field Notes.
* [GET /v3/education/user_reviews/{id}](#api-user-reviews-item) - Get a specified product Field Note.
* [GET /v3/education/users](#api-users-list) - Get a list of users.
* [GET /v3/education/users/{id}](#api-users-item) - Get a specified user.
* [GET /v3/education/search/products/{query}](#api-search-products) - Search for products.
* [GET /v3/education/search/flows/{query}](#api-search-flows) - Search for Lesson Flows.
* [GET /v3/education/search/blogs/{query}](#api-search-blogs) - Search for Blog posts.
* [GET /v3/education/search/lists/{query}](#api-search-lists) - Search for Top Picks lists.
* [GET /v3/education/search/boards/{query}](#api-search-boards) - Search for Boards.
* [GET /v3/education/search/user_reviews/{query}](#api-search-user-reviews) - Search for Field Notes.
* [GET /v3/education/search/users/{query}](#api-search-users) - Search for users.

---

# Overview

<a name="api-url-patterns"></a>
URL Patterns
--------------------

Generically in this documentation URLs will use https://graphite-api.commonsense.org as the canonical reference for documentation.
However, it should be noted that the alternate form is https://graphite-api-{ENV}.commonsense.org where ENV can be `qa` or `dev`.

<a name="api-retrieving-resources"></a>
Retrieving Resources
--------------------

Retrieving resources with the HTTP GET method is as simple as GETting its URL. GET requests data from a specified resource. Key/value pairs should be URL-encoded and sent in the URL of a GET request:

### Example

<pre>
curl -X GET "https://graphite-api.commonsense.org/v3/example/endpoint?foo=bar&hello=world&appId=abc123&clientId=xyz456"
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

<a name="api-request-authentication"></a>
Request Authentication
----------------------

Each REST API call is to send a valid `app_id` and `token` combination in the query string.

### Example

<pre>
curl -X GET "https://graphite-api.commonsense.org/v3/education/products?appId=abc123&clientId=xyz456"
</pre>

---
# Endpoints

<a name="api-products-list"></a>
GET /v3/education/products
--------------------------

Get a list of products.

### Query Parameters

* `ids` - A comma separated list of product IDs to be retrieved.
  * default: all records returned.
* `fields` - A comma separated list of fields to be outputted.
  * default: all data fields
* `page` - The page offset of the data set.
  * default: `1`
* `limit` - The number of records to be outputted per page.
  * default: `10`
* `sort` - A comma separated list of fields, each with a possible unary negative to imply descending sort order.
  * default: `-created` *(decending)*
* `hasPolicyUrl` - Boolean `true` or `false` that filters whether the products have a privacy policy URL.
  * default: all records returned.

### Example

<pre>
curl -X GET "https://graphite-api.commonsense.org/v3/education/products?appId=abc123&clientId=xyz456"
</pre>

### Output
An array of product objects.

<a name="product-object"></a>
### Product Object Sample

<pre>
{
  "id":1247882,
  "title":"Minecraft",
  "status":1,
  "type":"game",
  "created":"2011-04-12T16:51:00.000Z",
  "changed":"2015-01-12T23:30:17.000Z",
  "user_id":284418,
  "url":"https://www.graphite.org/node/1247882",
  "image":"https://d2gtfp3wq3fbfv.cloudfront.net/sites/default/files/experience-media-file/minecraft_0_0.jpg",
  "release_date":"2011-04-11T07:00:00.000Z",
  "price":"21.00",
  "pricing_details":null,
  "subscription_price":null,
  "esrb_explanation":"Fantasy Violence",
  "policy_urls":[
    "https://www.minecraft.net/policy",
  ],
  "platforms":[
    {
      "name":"Linux",
      "parent_id":0,
      "id":3996
    },
    ...
  ],
  "esrb_ratings":[
    {
      "name":"E10+",
      "parent_id":0,
      "id":3877
    },
    ...
  ],
  "pricing_structure":[
    {
      "name":"Paid",
      "parent_id":0,
      "id":19599
    },
    ...
  ],
  "awards":[],
  "publisher":{
    "parent_tid":0,
    "name":"Mojang",
    "tid":4593
  },
  "genre":{
    "parent_tid":0,
    "name":"Adventure",
    "tid":3868
  },
  "screenshots":[
    "https://d2gtfp3wq3fbfv.cloudfront.net/sites/default/files/screenshots/csm-game/mine.jpg",
    "https://d2gtfp3wq3fbfv.cloudfront.net/sites/default/files/screenshots/csm-game/mine-2.jpg",
    "https://d2gtfp3wq3fbfv.cloudfront.net/sites/default/files/screenshots/csm-game/mine1.jpg"
  ],
  "available_on":[],
  "common_core_english":[],
  "common_core_math":[],
  "grades":[
    {
      "name":"3",
      "parent_id":0,
      "id":21930
    },
    ...
  ],
  "population":[
    {
      "name":"General",
      "parent_id":0,
      "id":21958
    },
    ...
  ],
  "purpose":[
    {
      "name":"Knowledge gain",
      "parent_id":0,
      "id":21948
    },
    ...
  ],
  "skills":[
    {
      "name":"Communication & Collaboration",
      "parent_id":0,
      "id":19651
    },
    ...
  ],
  "subjects":[],
  "review":{
    "privacy_information":[],
    "learning_rating":5,
    "tags":"storytelling, geometry, shapes, ecosystems, geology, substance properties, citizenship, geography, coding, digital creation, playing, sculpture, game design, project-based learning ",
    "is_subreview":false,
    "is_coming_soon":false,
    "good_for_learning":"<p>Since each new world begs to be explored and reshaped, <em>Minecraft</em> cultivates 21<sup>st</sup>-century skills: goal-setting, collaboration, creativity, design and systems thinking, and engineering. Teachers should be aware, however, that the game’s emphasis on open creation, collaboration, and communication also means that students playing together can get into conflicts or get distracted and off task. If framed less as a problem and more as an opportunity, these issues can be made into powerful learning experiences that guide students toward successful and respectful collaboration.</p>\n",
    "what_kids_learn":null,
    "what_is_story":"<p><em>Minecraft</em> can be adapted to fit nearly any objective or subject, with lessons lasting as short as one period or the entire year. The game is so engrossing that students are likely to work long hours on projects both at home and school, so be wary of time management as well as issues -- like griefing -- that may arise from at-home and potentially unsupervised use. While playing in class, teachers can help students negotiate norms, roles, and responsibilities, and foster trust and a sense of consequence for individuals’ actions in community.</p>\n<p>Students can use <em>Minecraft </em>as a portfolio, creating structures and systems that model topics or concepts covered in class. In a math classroom, students can tackle problems using a set number of blocks (the basic unit of <em>Minecraft</em>) or calculate area and volume. For writing practice, students can keep explorers’ journals or compare and contrast the biomes and geologies of their <em>Minecraft</em> worlds with those of their home states.</p>\n",
    "learning_rating_total":5,
    "author":{
      "id":1855701,
      "name":"Chad Sansing",
      "bio":"<p>I teach language arts at a traditional, &quot;micropolitan&quot; middle school in Staunton, Virginia.</p>\n<p>I believe in making stuff with kids as a pathway to reading, writing, and problem-solving in community. I am co-founder and moderator of the collaborative progressive education blog CoöpCatalyst and I recently began work on #nerdcamp, which offers teachers peer-to-peer and student-mentored professional development in new media. I am a National Board Certified Teacher in early adolescence English/language Arts, a NETS*T certified teacher, a Mozilla Webmaker Mentor, and a National Writing Project teacher consultant and connected learning ambassador. I blog about make/hack/play education on my webpage, Classroots.org, and as a guest at sites like the National Writing Project&#039;s Digital Is.</p>\n<p>I live in central Virginia with my wife, our two kids, our two cats, and eight (so far) fish. When I&#039;m not teaching or blogging about education, I&#039;m noodling around with code, playing with the kids, or enjoying their soccer games.</p>\n",
      "profile_picture":"https://d2gtfp3wq3fbfv.cloudfront.net/sites/default/files/user-pictures/1855701-user-picture.jpg",
      "first_name":"Chad ",
      "last_name":"Sansing",
      ...
    },
    "tech_notes":"To download, players must register an account with Mojang (the game’s publisher). This registration requires an email.  The game downloads and installs in five minutes and requires the latest version of Java.  ",
    "teachers_need_to_know":"<p><em>Minecraft</em> is a sandbox game that rewards players for collecting and combining resources into new, useful items that enrich gameplay and help further exploration and creativity. Although it has an “End” zone for players who want to fight the game’s boss (a dragon), <em>Minecraft</em> has no plot -- the story is up to the player to define. Depending on what the player chooses to build, they’ll task themselves with collecting specific resources necessary to craft items that can help them build cooler and/or more useful things, or explore. Each completed project inevitably leads to a new one with new resource and item needs, sending the player deeper into the world. Selecting Creative Mode, as opposed to the default Survival Mode, removes the need to collect resources as well as the monsters and health and hunger meters, allowing players to build easily and in peace. Creative Mode is probably best for younger students who might get too distracted by monsters or lessons requiring complex builds in a short amount of time.</p>\n<p>To get started, players create accounts on <a href=\"http://www.minecraft.net\" target=\"_blank\">Minecraft.net</a>, purchase a license, and then download and install the game. Players can create a brand-new unique world on their own or join other people’s worlds (via local area network or hosted servers), fulfilling both solitary and social players. The controls are typical of first-person games using a combination of the mouse and WASD keys.</p>\n",
    "one_liner":"Spiraling sandbox of adventure and creation gets kids to dig deep",
    "learning_approach_rating":5,
    "learning_approach_description":"<p>Design thinking, problem solving, and resilience will stay with students, but specific content-knowledge transfer is dependent on how classes use the game.</p>\n",
    "how_parents_help":null,
    "how_kids_learn":null,
    "help_rating":4,
    "help_description":"<p>Lacking a built-in tutorial or manual, &lt;i&gt;Minecraft&lt;/i&gt; can be intimidating, but this also promotes peer learning both among students and the larger online community.</p>\n",
    "engagement_rating":5,
    "engagement_description":"<p>Students have free reign over one-of-a-kind worlds bolstered by deep customization options and frequent updates that add new challenges and content.</p>\n",
    "educational_intent":0,
    "bottom_line":"An irresistible and seemingly limitless incubator for 21st-century skills that, with a little guidance, can chart new courses for learning.",
    "cons":"Open world can lead to power struggles and community problems without a shared code of conduct.",
    "pros":"Delivers open, creative, and purposeful play supported by frequent updates.",
    "user_rating_count":13,
    "user_rating_average":4.15,
    "admin_tools":"<p>N/A</p>\n",
    "consumer_product_id":1247882,
    "product_id":1247882,
    "url":"https://www.graphite.org/game/minecraft",
    "user_id":1855701,
    "changed":"2015-04-13T18:17:23.000Z",
    "created":"2012-03-30T00:03:00.000Z",
    "type":"learning_rating",
    "status":1,
    "title":"Minecraft",
    "id":2864106
  }
}
</pre>


<a name="api-products-item"></a>
GET /v3/education/products/{id}
-------------------------------

Get a specified product.

### Path Parameters

* `id` - The system ID of a product.

### Query Parameters

* `fields` - A comma separated list of fields to be outputted.
  * default: all data fields

### Example

<pre>
curl -X GET "https://graphite-api.commonsense.org/v3/education/products/1247882?appId=abc123&clientId=xyz456"
</pre>

### Output

A <a href="#product-object">product object</a>.

<a name="api-flows-list"></a>
GET /v3/education/flows
-----------------------

Get a list of Lesson Flows.

### Query Parameters

* `ids` - A comma separated list of Lesson Flow IDs to be retrieved.
  * default: all records returned.
* `fields` - A comma separated list of fields to be outputted.
  * default: all data fields
* `page` - The page offset of the data set.
  * default: `1`
* `limit` - The number of records to be outputted per page.
  * default: `10`
* `sort` - A comma separated list of fields, each with a possible unary negative to imply descending sort order.
  * default: `-created` *(decending)*

### Example

<pre>
curl -X GET "https://graphite-api.commonsense.org/v3/education/flows?appId=abc123&clientId=xyz456"
</pre>

### Output

An array of Lesson Flow objects.

<a name="lesson-flow-object"></a>
### Lesson Flow Object Sample

<pre>
[
  {
    "id":3898001,
    "title":"Polygons",
    "status":1,
    "type":"flow",
    "created":"2013-09-17T17:28:00.000Z",
    "changed":"2015-03-10T23:58:40.000Z",
    "user_id":1536766,
    "url":"https://www.graphite.org/lesson-flows/polygons",
    "description":"An introduction to some basic geometric figures and their place in the real world",
    "subjects":[
      {
        "name":"Math",
        "parent_id":0,
        "id":19647
      },
      ...
    ],
    "grades":[
      {
        "name":"3",
        "parent_id":0,
        "id":21930
      },
      ...
    ],
    "usefulness":0,
    "components":[
      {
        "title":"Direct Instruction",
        "description":"<p>Use <strong>BrainPOP’s “<em>Polygons</em>”</strong> video to begin outlining today’s discussion. Next, define the characteristics of a “polygon”:</p>\n<p><em>Polygon</em> –</p>\n<ul><li>Has at least 3 sides (known as line segments or edges) and angles</li>\n<li>Is a closed plane (2D) figure</li>\n<li>Has straight line segments (no curves!)</li>\n</ul><p>Draw three basic polygons as examples (a triangle and two quadrilaterals) and three non-polygons (circle, key shape, heart). Ask student to vote as to whether each shape fulfills the criteria outlined in the definition of a polygon. Begin categorizing the polygon examples you drew by number of sides, and continue: trigon/triangle, tetragon/quadrilateral, pentagon, hexagon, heptagon/septagon, octagon, enneagon/nonagon, decagon. Where possible, highlight examples of each type of polygon found in the real world (triangle = pizza slice, pentagon = home plate, octagon = stop sign). Introduce new math terminology by discussing how polygons are classified: regular v. irregular, simple v. complex (self-intersecting), convex v. concave.</p>\n<p>(Other related vocabulary: vertex/vertices, edge, line segment, angle, equilateral, equiangular)</p>\n",
        "instructions":"",
        "products":[
          {
            "id":3588336,
            "title":"BrainPop",
            "status":1,
            "type":"website",
            "created":"2012-11-13T02:01:32.000Z",
            "changed":"2015-01-12T18:25:46.000Z",
            ...
          }
        ],
        "activity":[],
        "activity_other":null
      },
      ...
    ],
    "author":{
      "id":1536766,
      "bio":"<p>My current role at Common Sense as director of digital learning melds my love of instructional design, writing, and the ever-changing ed-tech world. I have taught in Los Angeles and New York City public schools for over ten years, and I have worked for education-focused media companies such as Nickelodeon, IMAX, EdSurge, and Discovery Education. I'm passionate about creative curriculum development that will push the boundaries of current pedagogy.</p>\n",
      "profile_picture":"https://d2gtfp3wq3fbfv.cloudfront.net/sites/default/files/user-pictures/1536766-user-picture.jpg",
      "first_name":"Darri",
      "last_name":"Stephens",
      ...
    }
  },
  ...
]
</pre>


<a name="api-flows-item"></a>
GET /v3/education/flows/{id}
----------------------------

Get a specified Lesson Flow.

### Path Parameters

* `id` - The system ID of a Lesson Flow.

### Query Parameters

* `fields` - A comma separated list of fields to be outputted.
  * default: all data fields

### Example

<pre>
curl -X GET "https://graphite-api.commonsense.org/v3/education/flows/3898001?appId=abc123&clientId=xyz456"
</pre>

### Output

A <a href="#lesson-flow-object">Lesson Flow</a> object.

<a name="api-blogs-list"></a>
GET /v3/education/blogs
--------------------------

Get a list of blog posts.

### Query Parameters

* `ids` - A comma separated list of blog IDs to be retrieved.
  * default: all records returned.
* `fields` - A comma separated list of fields to be outputted.
  * default: all data fields
* `page` - The page offset of the data set.
  * default: `1`
* `limit` - The number of records to be outputted per page.
  * default: `10`
* `sort` - A comma separated list of fields, each with a possible unary negative to imply descending sort order.
  * default: `-created` *(decending)*

### Example

<pre>
curl -X GET "https://graphite-api.commonsense.org/v3/education/blogs?appId=abc123&clientId=xyz456"
</pre>

### Output
An array of blog post objects.

<a name="blog-object"></a>
### Blog Post Object Sample

<pre>
{
  "id":3899826,
  "title":"Minecraft or MinecraftEdu at School? Pros, Cons, and What it's Great For",
  "status":1,
  "type":"blog",
  "created":"2013-09-25T04:52:00.000Z",
  "changed":"2014-09-30T22:10:01.000Z",
  "user_id":1855701,
  "url":"https://www.graphite.org/blog/minecraft-or-minecraftedu-at-school-pros-cons-and-what-its-great-for",
  "subheader":null,
  "body":"<p>One of the great things about Minecraft is that there's not just one version. It exists as a stand-alone game, but creative players also create modifications, or \"mods,\" that add all sorts of upgrades, features, and options for every taste. One of the most popular mods for classrooms is MinecraftEdu....</p>",
  "image":"https://d2gtfp3wq3fbfv.cloudfront.net/sites/default/files/tlr_board/post/kidsplayingminecraftatschool.jpg",
  "grades":[
    {
      "name":"3",
      "parent_id":0,
      "id":21930
    },
    ...
  ],
  "subjects":[],
  "skills":[],
  "topics":[
    {
      "tid":21975,
      "name":"Tools",
      "parent_tid":0
    },
    ...
  ],
  "post_types":[
    {
      "name":"Tech Sketch",
      "parent_id":0,
      "id":21972
    },
    ...
  ],
  "author":{
    "id":1855701
    "bio":"<p>I teach language arts at a traditional, &quot;micropolitan&quot; middle school in Staunton, Virginia.</p>\n<p>I believe in making stuff with kids as a pathway to reading, writing, and problem-solving in community. I am co-founder and moderator of the collaborative progressive education blog CoöpCatalyst and I recently began work on #nerdcamp, which offers teachers peer-to-peer and student-mentored professional development in new media. I am a National Board Certified Teacher in early adolescence English/language Arts, a NETS*T certified teacher, a Mozilla Webmaker Mentor, and a National Writing Project teacher consultant and connected learning ambassador. I blog about make/hack/play education on my webpage, Classroots.org, and as a guest at sites like the National Writing Project&#039;s Digital Is.</p>\n<p>I live in central Virginia with my wife, our two kids, our two cats, and eight (so far) fish. When I&#039;m not teaching or blogging about education, I&#039;m noodling around with code, playing with the kids, or enjoying their soccer games.</p>\n",
    "profile_picture":"https://d2gtfp3wq3fbfv.cloudfront.net/sites/default/files/user-pictures/1855701-user-picture.jpg",
    "first_name":"Chad",
    "last_name":"Sansing",
    ...
  }
}
</pre>


<a name="api-blogs-item"></a>
GET /v3/education/blogs/{id}
-------------------------------

Get a specified blog post.

### Path Parameters

* `id` - The system ID of a blog post.

### Query Parameters

* `fields` - A comma separated list of fields to be outputted.
  * default: all data fields

### Example

<pre>
curl -X GET "https://graphite-api.commonsense.org/v3/education/blogs/3899826?appId=abc123&clientId=xyz456"
</pre>

### Output

A <a href="#blog-object">blog post object</a>.

<a name="api-lists-list"></a>
GET /v3/education/lists
--------------------------

Get a list of Top Picks lists.

### Query Parameters

* `ids` - A comma separated list of Top Pick List IDs to be retrieved.
  * default: all records returned.
* `fields` - A comma separated list of fields to be outputted.
  * default: all data fields
* `page` - The page offset of the data set.
  * default: `1`
* `limit` - The number of records to be outputted per page.
  * default: `10`
* `sort` - A comma separated list of fields, each with a possible unary negative to imply descending sort order.
  * default: `-created` *(decending)*

### Example

<pre>
curl -X GET "https://graphite-api.commonsense.org/v3/education/lists?appId=abc123&clientId=xyz456"
</pre>

### Output
An array of Top Pick List objects.

<a name="list-object"></a>
### Top Picks List Object Sample

<pre>
{
  "id":3887113,
  "title":"Best Tech Creation Tools",
  "status":1,
  "type":"top_picks",
  "created":"2013-03-26T21:03:00.000Z",
  "changed":"2015-05-02T04:52:41.000Z",
  "user_id":343980,
  "url":"https://www.graphite.org/top-picks/best-tech-creation-tools",
  "teaser":"Put a personal spin on learning through invention.",
  "overview":null,
  "sections":[
    {
      "title":null,
      "products":[
        {
          "id":1242472,
          "title":"Scratch",
          "status":1,
          "type":"website",
          "created":"2011-02-08T20:39:34.000Z",
          "changed":"2015-03-13T17:11:27.000Z",
          ...
        },
        ...
      ]
    }
  ],
  "skills":[
    {
      "name":"Communication & Collaboration",
      "parent_id":0,
      "id":19651
    },
    ...
  ],
  "grades":[],
  "subjects":[],
  "author":{
    "id":343980,
    "bio":"<p>As Common Sense Education's senior director of education content, Shira is responsible for the strategic direction and overall quality of content on Common Sense Education's Graphite (<a href=\"http://www.graphite.org\">www.graphite.org</a>). Graphite is an innovative platform for PreK-12 teachers with independent ratings and reviews of EdTech. Previously, Shira oversaw the direction and operations of Common Sense's app, website, and game channels and project managed the research and creation of our K-12 digital literacy and citizenship curriculum and companion online interactive elements. Shira has also managed research projects on digital learning, school reform, and social emotional development. More generally, Shira is interested in seeding innovative approaches to learning. Creation games like <i>Minecraft</i> and offbeat TV comedies like <i>Arrested Development</i> occupy far too much of Shira's time. She holds a doctorate degree from the Harvard Graduate School of Education and a bachelor's in English from the University of Michigan.<br /><a href=\"https://plus.google.com/117799627444797092612/posts\" rel=\"me\">Google+</a></p>\n",
    "profile_picture":"https://d2gtfp3wq3fbfv.cloudfront.net/sites/default/files/user-pictures/343980-user-picture.jpg",
    "first_name":"Shira Lee",
    "last_name":"Katz",
    ...
  },
}
</pre>


<a name="api-lists-item"></a>
GET /v3/education/lists/{id}
-------------------------------

Get a specified Top Picks List post.

### Path Parameters

* `id` - The system ID of a Top Picks List.

### Query Parameters

* `fields` - A comma separated list of fields to be outputted.
  * default: all data fields

### Example

<pre>
curl -X GET "https://graphite-api.commonsense.org/v3/education/lists/3887113?appId=abc123&clientId=xyz456"
</pre>

### Output

A <a href="#list-object">Top Picks List object</a>.


<a name="api-boards-list"></a>
GET /v3/education/boards
--------------------------

Get a list of Boards.

### Query Parameters

* `ids` - A comma separated list of Board IDs to be retrieved.
  * default: all records returned.
* `fields` - A comma separated list of fields to be outputted.
  * default: all data fields
* `page` - The page offset of the data set.
  * default: `1`
* `limit` - The number of records to be outputted per page.
  * default: `10`
* `sort` - A comma separated list of fields, each with a possible unary negative to imply descending sort order.
  * default: `-created` *(decending)*

### Example

<pre>
curl -X GET "https://graphite-api.commonsense.org/v3/education/boards?appId=abc123&clientId=xyz456"
</pre>

### Output
An array of Board objects.

<a name="board-object"></a>
### Board Object Sample

<pre>
{
  "id":3961626,
  "title":"Writing, Storytelling, Self-Publishing",
  "status":1,
  "type":"board",
  "created":"2014-05-07T19:07:43.000Z",
  "changed":"2014-05-07T19:37:37.000Z",
  "user_id":1473181,
  "url":"https://www.graphite.org/users/erin-wilkey-oh/boards/writing-storytelling-self-publishing",
  "description":"",
  "notes":null,
  "grades":[],
  "subjects":[],
  "posts":[
    {
      "id":3894861,
      "title":"Write About This",
      "status":1,
      "type":"csm_learning_rating",
      "created":1378311360,
      "changed":1428466637,
      "user_id":1746446,
      "url":"https://www.graphite.org/app/write-about-this",
      "description":"Fun pictures and prompts are great for getting kids to start writing",
      "image":"https://d2gtfp3wq3fbfv.cloudfront.net/sites/default/files/experience-media-file/screen_shot_2015-03-21_at_5.04.41_pm.png",
      "post_type":"learning_rating"
    },
    ...
  ],
  "author":{
    "id":1473181,
    "bio":"<p>Before joining Common Sense Education as senior editor of education reviews, Erin spent several years teaching English for non-native speakers at a community college and 11th/12th-grade English at an urban public high school. Her school district&#039;s 1:1 laptop program sparked her interest in what teaching and learning could look like in a connected classroom. Erin has bachelor&#039;s degrees in English and secondary education and a master&#039;s degree in instructional design and technology. She also explored the educational potential of new media as a writer and curator for the National Writing Project&#039;s Digital Is website and an app reviewer for YogiPlay. Erin is a knitter, DIY enthusiast, and occasional food blogger. As a new parent, she&#039;s learning a ton about early childhood development and is having a blast witnessing the endless curiosity of her infant son.</p>\n",
    "profile_picture":"https://d2gtfp3wq3fbfv.cloudfront.net/sites/default/files/user-pictures/1473181-user-picture_0.jpg",
    "first_name":"Erin Wilkey",
    "last_name":"Oh",
    ...
  },
}
</pre>


<a name="api-boards-item"></a>
GET /v3/education/boards/{id}
-------------------------------

Get a specified Board.

### Path Parameters

* `id` - The system ID of a Board.

### Query Parameters

* `fields` - A comma separated list of fields to be outputted.
  * default: all data fields

### Example

<pre>
curl -X GET "https://graphite-api.commonsense.org/v3/education/boards/3961626?appId=abc123&clientId=xyz456"
</pre>

### Output

A <a href="#board-object">Board object</a>.

<a name="api-user-reviews-list"></a>
GET /v3/education/user_reviews
--------------------------

Get a list of Field Notes.

### Query Parameters

* `ids` - A comma separated list of Field Note IDs to be retrieved.
  * default: all records returned.
* `fields` - A comma separated list of fields to be outputted.
  * default: all data fields
* `page` - The page offset of the data set.
  * default: `1`
* `limit` - The number of records to be outputted per page.
  * default: `10`
* `sort` - A comma separated list of fields, each with a possible unary negative to imply descending sort order.
  * default: `-created` *(decending)*

### Example

<pre>
curl -X GET "https://graphite-api.commonsense.org/v3/education/user_reviews?appId=abc123&clientId=xyz456"
</pre>

### Output
An array of Field Note objects.

<a name="user-review-object"></a>
### Field Note Object Sample

<pre>
{
  "id":3963361,
  "title":"Minecraft for Schools Made Easy",
  "status":1,
  "type":"field_note",
  "created":"2014-05-13T17:12:42.000Z",
  "changed":"2014-06-04T14:47:08.000Z",
  "url":"https://www.graphite.org/game/minecraftedu-teacher-review/3963361",
  "product_id":3888265,
  "engagement_rating":5,
  "pedagogy_rating":4,
  "support_rating":5,
  "learning_rating":5,
  "setup_time":"gt15",
  "review":"<p>Once you can get the software up and running, it&#039;s easy. There is a great Google Group community called &quot;Minecraft Teachers&quot; that are extremely helpful and willing to answer any questions you may have. The biggest hurdle for me, as a teacher, is making sure students understand the difference between how they play Minecraft at home vs. how they are expected to play in school. Many students simply want to kill or be killed when they play at home, and my purpose for using Minecraft in school is very different. I want to get students to cooperate and collaborate with each other, solve problems, and be creative instead of &quot;griefing&quot; each other. Overall, the fact that I&#039;m allowing students to use Minecraft in school has been highly motivating and engaging for ALL students.</p>\n",
  "experience":"<p>The Minecraft EDU version of Minecraft includes a wide variety of tools for teachers to use in the classroom. I&#039;ve used it to teach Digital Citizenship, Social Studies, and Language Arts. Examples of projects I&#039;ve used with grades 1-5 include:<br />\n5th Grade: creating a replica of Jamestown Colony<br />\n4th Grade: create a mining town in Colorado<br />\n3rd Grade: create an Anasazi Cliff Dwelling<br />\n2nd and 1st Grade: create buildings in a community<br />\nThe possibilities are endless. Further ideas I plan to use:  create a setting from a book that students have read, reinforce area and perimeter, construct geometric shapes, build circuits with redstone.</p>\n",
  "purpose":[
    {
      "name":"Creation",
      "parent_id":0,
      "id":21951
    },
    ...
  ],
  "population":[
    {
      "name":"General",
      "parent_id":0,
      "id":21958
    },
    ...
  ]
  "product":{
    "id":3888265,
    "title":"MinecraftEdu",
    "esrb_explanation":"Not rated by the ESRB",
    "subscription_price":null,
    "pricing_details":"",
    "price":"$41.00",
    "release_date":"2013-04-15T07:00:00.000Z",
    "image":"https://d2gtfp3wq3fbfv.cloudfront.net/sites/default/files/experience-media-file/minecraft-edu_0.jpg",
    "changed":"2015-04-13T18:00:02.000Z",
    "created":"2013-05-07T22:58:00.000Z",
    ...
    "review":{
      "id":3888432,
      "status":1,
      "type":"learning_rating",
      "created":"2013-05-07T22:59:00.000Z",
      "changed":"2015-04-22T19:07:09.000Z",
      "url":"https://www.graphite.org/game/minecraftedu",
      ...
    },
  },
  "author":{
    "id":2169956,
    "bio":"<p>Technology Specialist at Village East Elementary.</p>\n",
    "profile_picture":"https://d2gtfp3wq3fbfv.cloudfront.net/sites/default/files/user-pictures/2169956-user-picture.jpg",
    "first_name":"Joel",
    "last_name":"Solomon",
    ...
  },
}
</pre>

<a name="api-user-reviews-item"></a>
GET /v3/education/user_reviews/{id}
-----------------------------------

Get a specified Field Note.

### Path Parameters

* `id` - The system ID of a Field Note.

### Query Parameters

* `fields` - A comma separated list of fields to be outputted.
  * default: all data fields

### Example

<pre>
curl -X GET "https://graphite-api.commonsense.org/v3/education/user_reviews/3963361?appId=abc123&clientId=xyz456"
</pre>

### Output

A <a href="#user-review-object">Field Note object</a>.


<a name="api-users-list"></a>
GET /v3/education/users
-----------------------

Get a list of Users.

### Query Parameters

* `ids` - A comma separated list of user IDs to be retrieved.
  * default: all records returned.
* `fields` - A comma separated list of fields to be outputted.
  * default: all data fields.
* `page` - The page offset of the data set.
  * default: `1`
* `limit` - The number of records to be outputted per page.
  * default: `10`
* `sort` - A comma separated list of fields, each with a possible unary negative to imply descending sort order.
  * default: `-created` *(decending)*
* `has_school` - Boolean `true` or `false` that filters whether the users have a school association.
  * default: all records returned.
* `school_state` - A 2 letter abbreviation of a school's state/province to filter by *(example: **CA** for California)*.
    * default: all records returned.

### Example

<pre>
curl -X GET "https://graphite-api.commonsense.org/v3/education/user?appId=abc123&clientId=xyz456"
</pre>

### Output
An array of User objects.

<a name="user-object"></a>
### User Object Sample

<pre>
{
  "id":1473181,
  "bio":"<p>Before joining Common Sense Education as senior editor of education reviews, Erin spent several years teaching English for non-native speakers at a community college and 11th/12th-grade English at an urban public high school. Her school district&#039;s 1:1 laptop program sparked her interest in what teaching and learning could look like in a connected classroom. Erin has bachelor&#039;s degrees in English and secondary education and a master&#039;s degree in instructional design and technology. She also explored the educational potential of new media as a writer and curator for the National Writing Project&#039;s Digital Is website and an app reviewer for YogiPlay. Erin is a knitter, DIY enthusiast, and occasional food blogger. As a new parent, she&#039;s learning a ton about early childhood development and is having a blast witnessing the endless curiosity of her infant son.</p>\n",
  "profile_picture":"https://d2gtfp3wq3fbfv.cloudfront.net/sites/default/files/user-pictures/1473181-user-picture_0.jpg",
  "first_name":"Erin Wilkey",
  "last_name":"Oh",
  "display_name":"Erin Wilkey O.",
  "full_name":"Erin Wilkey Oh",
  "status":1,
  "role_title":null,
  "role_title_other":null,
  "teaching_since":null,
  "school_tech_profile":null,
  "profile_url":"https://www.graphite.org/users/erin-wilkey-oh",
  "is_homeschooler":false,
  "is_educator":false,
  "is_parent":false,
  "is_expert_educator":false,
  "is_fully_registered":false,
  "is_admin":false,
  "filtered_roles":[
    {
      "name":"Graphite Reviewer",
      "parent_id":0
    },
    ...
  ],
  "grades":[],
  "subjects":[],
  "created":"2012-04-21T00:22:04.000Z",
  "login":"2015-05-01T14:36:58.000Z",
  "school":{
    "name":null,
    "city":null,
    "state":null
  }
}
</pre>


<a name="api-users-item"></a>
GET /v3/education/users/{id}
----------------------------

Get a specified User.

### Path Parameters

* `id` - The system ID of a User.

### Query Parameters

* `fields` - A comma separated list of fields to be outputted.
  * default: all data fields

### Example

<pre>
curl -X GET "https://graphite-api.commonsense.org/v3/education/users/1247882?appId=abc123&clientId=xyz456"
</pre>

### Output

A <a href="#user-object">User object</a>.

<a name="api-search-products"></a>
GET /v3/education/search/products/{query}
-----------------------------------------

Search for products.

### Path Parameters

* `query` - The search string.

### Query Parameters

* `type` - The type of product to filter by.
  * default: all data fields
  * values: `app`, `game`, `website`, `movie`, `book`, `music`, or `tv`
* `fields` - A comma separated list of fields to be outputted.
  * default: all data fields
* `page` - The page offset of the data set.
  * default: `1`
* `limit` - The number of records to be outputted per page.
  * default: `10`

### Example

<pre>
curl -X GET "https://graphite-api.commonsense.org/v3/search/products/math?type=app&appId=abc123&clientId=xyz456"
</pre>

### Output

An array of <a href="#product-object">product objects</a>.

<a name="api-search-flows"></a>
GET /v3/education/search/flows/{query}
-----------------------------------------

Search for Lesson Flows.

### Path Parameters

* `query` - The search string.

### Query Parameters

* `fields` - A comma separated list of fields to be outputted.
  * default: all data fields
* `page` - The page offset of the data set.
  * default: `1`
* `limit` - The number of records to be outputted per page.
  * default: `10`

### Example

<pre>
curl -X GET "https://graphite-api.commonsense.org/v3/search/products/math?appId=abc123&clientId=xyz456"
</pre>

### Output

An array of <a href="#lesson-flow-object">Lesson Flow objects</a>.

<a name="api-search-blogs"></a>
GET /v3/education/search/blogs/{query}
-----------------------------------------

Search for Blog posts.

### Path Parameters

* `query` - The search string.

### Query Parameters

* `fields` - A comma separated list of fields to be outputted.
  * default: all data fields
* `page` - The page offset of the data set.
  * default: `1`
* `limit` - The number of records to be outputted per page.
  * default: `10`

### Example

<pre>
curl -X GET "https://graphite-api.commonsense.org/v3/search/blogs/math?appId=abc123&clientId=xyz456"
</pre>

### Output

An array of <a href="#blog-object">Blog Post objects</a>.

<a name="api-search-lists"></a>
GET /v3/education/search/lists/{query}
-----------------------------------------

Search for Top Picks Lists.

### Path Parameters

* `query` - The search string.

### Query Parameters

* `fields` - A comma separated list of fields to be outputted.
  * default: all data fields
* `page` - The page offset of the data set.
  * default: `1`
* `limit` - The number of records to be outputted per page.
  * default: `10`

### Example

<pre>
curl -X GET "https://graphite-api.commonsense.org/v3/search/lists/math?appId=abc123&clientId=xyz456"
</pre>

### Output

An array of <a href="#list-object">Top Picks List objects</a>.

<a name="api-search-boards"></a>
GET /v3/education/search/boards/{query}
-----------------------------------------

Search for Boards.

### Path Parameters

* `query` - The search string.

### Query Parameters

* `fields` - A comma separated list of fields to be outputted.
  * default: all data fields
* `page` - The page offset of the data set.
  * default: `1`
* `limit` - The number of records to be outputted per page.
  * default: `10`

### Example

<pre>
curl -X GET "https://graphite-api.commonsense.org/v3/search/boards/math?appId=abc123&clientId=xyz456"
</pre>

### Output

An array of <a href="#board-object">Board objects</a>.

<a name="api-search-user-reviews"></a>
GET /v3/education/search/user_reviews/{query}
-----------------------------------------

Search for Field Notes.

### Path Parameters

* `query` - The search string.

### Query Parameters

* `fields` - A comma separated list of fields to be outputted.
  * default: all data fields
* `page` - The page offset of the data set.
  * default: `1`
* `limit` - The number of records to be outputted per page.
  * default: `10`

### Example

<pre>
curl -X GET "https://graphite-api.commonsense.org/v3/search/user_reviews/math?appId=abc123&clientId=xyz456"
</pre>

### Output

An array of <a href="#user-review-object">Field Note objects</a>.

<a name="api-search-users"></a>
GET /v3/education/search/users/{query}
-----------------------------------------

Search for Users.

### Path Parameters

* `query` - The search string.

### Query Parameters

* `fields` - A comma separated list of fields to be outputted.
  * default: all data fields
* `page` - The page offset of the data set.
  * default: `1`
* `limit` - The number of records to be outputted per page.
  * default: `10`

### Example

<pre>
curl -X GET "https://graphite-api.commonsense.org/v3/search/users/john?appId=abc123&clientId=xyz456"
</pre>

### Output

An array of <a href="#user-object">User objects</a>.
