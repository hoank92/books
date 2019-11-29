# View Source

## I\) Table of Contents

## II\) Document Revision History

<table>
  <thead>
    <tr>
      <th style="text-align:left">No.</th>
      <th style="text-align:left">Modified At</th>
      <th style="text-align:left">Modified By</th>
      <th style="text-align:left">Detail</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">1</td>
      <td style="text-align:left">23/09/2019</td>
      <td style="text-align:left">anh.nguyen19@tiki.vn</td>
      <td style="text-align:left">support information, create &amp; tracking API</td>
    </tr>
    <tr>
      <td style="text-align:left">2</td>
      <td style="text-align:left">07/10/2019</td>
      <td style="text-align:left">anh.nguyen19@tiki.vn</td>
      <td style="text-align:left">add rate limit for API</td>
    </tr>
    <tr>
      <td style="text-align:left">3</td>
      <td style="text-align:left">14/10/2019</td>
      <td style="text-align:left">anh.nguyen19@tiki.vn</td>
      <td style="text-align:left">support create new product</td>
    </tr>
    <tr>
      <td style="text-align:left">4</td>
      <td style="text-align:left">15/10/2019</td>
      <td style="text-align:left">anh.nguyen19@tiki.vn</td>
      <td style="text-align:left">
        <p>support update price/quantity/price of created product</p>
        <p>create product support variant have its own market_price</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">5</td>
      <td style="text-align:left">28/10/2019</td>
      <td style="text-align:left">hoa.nguyen2@tiki.vn</td>
      <td style="text-align:left">add order endpoint</td>
    </tr>
    <tr>
      <td style="text-align:left">6</td>
      <td style="text-align:left">31/10/2019</td>
      <td style="text-align:left">anh.nguyen19@tiki.vn</td>
      <td style="text-align:left">add filter parameter for warehouse endpoint</td>
    </tr>
    <tr>
      <td style="text-align:left">7</td>
      <td style="text-align:left">07/11/2019</td>
      <td style="text-align:left"><a href="mailto:hoa.nguyen2@tiki.vn">hoa.nguyen2@tiki.vn</a>
      </td>
      <td style="text-align:left">add tracking_number for confirmItems endpoint</td>
    </tr>
    <tr>
      <td style="text-align:left">8</td>
      <td style="text-align:left">12/11/2019</td>
      <td style="text-align:left">hoa.nguyen2@tiki.vn</td>
      <td style="text-align:left">Add field coupon, discount in orders endpont</td>
    </tr>
    <tr>
      <td style="text-align:left">9</td>
      <td style="text-align:left">22/11/2019</td>
      <td style="text-align:left">hoa.nguyen2@tiki.vn</td>
      <td style="text-align:left">Add updateDeliveryStatus endpoint</td>
    </tr>
    <tr>
      <td style="text-align:left">10</td>
      <td style="text-align:left">26/11/2019</td>
      <td style="text-align:left"><a href="mailto:hoa.nguyen2@tiki.vn">hoa.nguyen2@tiki.vn</a>
      </td>
      <td style="text-align:left">Add field handling_fee, shipping_fee, inventory_type</td>
    </tr>
  </tbody>
</table>## III\) Introduction

TIKI Marketplace System is the platform for sellers to run marketplace business on TIKI Platform.Tiki integration system is a system which support seller to manage their product in Tiki dynamically .

It contains some kinds of APIs:

* Information APIs : provide Tiki's information such as : categories, attributes , rules, documents , ...
* Product APIs : create new product, update price, quantity , attributes, ...
* Tracking APIs : tracing request, product in history or by trace\_id
* Order APIs : to query, confirm order/delivery information

These APIs are opened and sellers can use these APIs to manage their business in TIKI platform

**\*Note**: update/delete API or any requested API will be supported in the near future 

## IV\) Authentication

Sellers has to use a secret token to integrate with our APIs.

The secret token is provided by the TIKI supporters or sellers can acquire in seller systems.

This token is used to access every APIs except Information APIs, it is opened for everybody.   

## V\) Product Endpoint

### 1\) Let's get started

Sellers can create **products** to sell on TIKI. A product can be sold by many sellers. Sellers offer their price and quantity for a product on TIKI.

There are two kinds of product at TIKI: simple product and variable products. 

* **Simple products** are the products that has attributes and only one instance/variant
* **Variable products** are the products that has many variants.

Variable products has many variants. Example: an iPhone has many variants differ by colors.

 They are called **option\_attributes.** Tiki support up to 2 option attributes \( size, color , capacity , ... \)

Each product belong to a specific **category** in a hierarchy. Example : iPhone belong to Cellphones → Smartphones → Apple → iPhone

Normally , only some category is **primary** that you can put products into it. In this case, the **primary category** is iPhone.

Each product has basic attributes: 

* Name: the name of product that is displayed on TIKI
* Price: the sell price of a product
* Market price : the price before discount of a product
* Description: describe the information of products
* Category: the **primary category** that products are belong. You must choose over Tiki Information APIs carefully
* Image: the avatar of product on TIKI
* Images: the image gallery of product on TIKI
* The other attributes are based on the category of products, like RAM/CPU/Screen. That's why you need to choose category carefully at first
* **Price** and **market price** use the **currency** which seller set with TIKI supporter when registry.

With variable products:

* A variable product has many **variants** and each variant maybe has its own attribute \(examples : name, color...\) 
* Variants differ by maximized two attributes. Example: a T Shirt has many variants that differ by color and size
* The attributes that are used to differentiate two variants, are named **option attributes**. Example a T Shirt differ two variants by color and size but a phone differ by RAM & screen size.

### 2\) Sequence diagram 

1. Client get/search tiki categories
2. Select tiki category, your product will map to tiki category
3. Using categoryId, get list attributes by categoryId
4. Mapping from your attribute to tiki attribute by **code**
5. After mapping success, you call API create product

### 3\) Entity

#### 3.1\) Category

| Field | Type | Example | Description |
| :--- | :--- | :--- | :--- |
| id | Integer | 218 | id of category |
| name | String | Medical Books | name of category |
| parent | Integer | 320 | category\_id of parent category |
| primary | Integer | 1 | only primary category can have product |

Example:

```text
{
    "id": 218,
    "name": "Medical Books",
    "parent": 320,
    "primary": 1
}
```

#### 3.2\) Attribute

| Field | Type | Example | Description |
| :--- | :--- | :--- | :--- |
| id | Integer | 498 | id of attribute |
| code | String | author | attribute code use to put into the attributes branch in payload |
| display\_name | String | Author | the display name of this attribute |
| is\_required | Integer | 1/0 | user must complete all of required attribute in payload |

Example:

```text
{
      "id": 498,
      "code": "author",
      "display_name": "Author",
      "is_required": 1
}
```

#### 3.3\) Product

| Field | Type | Mandatory | Description |
| :--- | :--- | :--- | :--- |
| category\_id | Integer | Y | TIKI categoryId you mapped before |
| name | String | Y | the name of product |
| description | String | Y | the description of product |
| market\_price | Integer | N | the price of product before discount |
| attributes | List&lt;Attribute&gt; | Y | list of attributes retrieve from category\_id  |
| image\(\*\) | String | Y | the avatar url of product |
| images\(\*\) | List&lt;String&gt; | Y | list urls of product gallery |
| option\_attributes\(\*\) | List&lt;String&gt; | Y | list of attribute code to config product \( up to 2 \) |
| variants | List&lt;Variant&gt; | Y | list of variants, simple product have only 1 |

Example:

```text
{
  "category_id": 1846,
  "name": "Laptop Dell Vostro 17 XPPP",
  "description": "this is description",
  "market_price": 12222222,
  "attributes": {
    "ram": "8GB",
    "ram_type": "Nvidia",
    "bus": "1333hz",
    "camera": "32MP",
    "audio_technology": "",
    "hard_drive": "1TB",
    "connection_port": "",
    "card_reader": "",
    "brand": "Dell Vostro"
  },
  "image": "https://www.laptopvip.vn/images/laptopvip_cache/e393b8aa5ec204efbea7582fc83a97ad/2/2/2249/original/2990378119/laptop-dell-xps-15-9570-05.png",
  "images": [
    "https://www.laptopvip.vn/images/companies/1/san-pham/untitled%20folder/dell%20xps%209570/800x530xdell-xps-15-2-in-1-review-front-display-1200x9999.jpg",
    "https://www.laptopvip.vn/images/companies/1/san-pham/untitled%20folder/dell%20xps%209570/x39b57caa-31ac-4872-9411-62bc6b15bffd.png"
  ],
  "option_attributes": [],
  "variants": [
    {
      "sku": "sku1",
      "price": 20000,
      "quantity": 20,
      "inventory_type" : "cross_border",
      "supplier" : 239091
    }
  ]
}
```

**\*Note**:

+ if product type is simple \( only one variant \) then **option\_attributes** must be empty list instead of null value because option attributes is a required field.

+ **images** do not accept null value, please put empty list if product don't have any image. The avatar from **image** will be added to **images** later so you don't need to add it 2 times.

Even you do that, we will check duplicate image by url.

\*For the best user experience, TIKI only display image have size greater than 500x500 pixel in the media gallery and lower than 700 width pixel inside description

#### 3.4\) Variant

| Field | Type | Mandatory | Override rule\(\*\) | Description |
| :--- | :--- | :--- | :--- | :--- |
| sku | String | Y | No | variant 's sku from source side |
| price | Integer | Y | No | variant 's sell price |
| market\_price | Integer | N | Replace | variant 's market price \( price before discount \) |
| option1 | String | N | No | attribute code of the first option attribute |
| option2 | String | N | No | attribute code of the first second attribute |
| inventory\_type\(\*\) | String | N | No | inventory type of this variant |
| quantity | Integer | Y | No | number of products available for sell |
| supplier\(\*\) | Integer | Y | No | see detail below |
| name | String | N | Replace | name of this variant |
| description | String | N | Replace | description of this variant |
| attributes | List&lt;Attribute&gt; | N | Merge  | list specific/addition attribute for this variant |
| image | String | N | Replace | avatar url of this variant |
| images | String | N | Replace | list urls of variant product gallery |

**\*Note**:

+ **option1**, **option2** is required corresponding with the number of option attributes start from 1.

The unused option value maybe null or empty or even don't need to appear.That's why it's mandatory still equal "no"  

+ **Override rule** describe how transform system will treat your request if any field is conflict between variant and parent product.

By default child product will inherit all of member from its parent.

* No : Field can't not override 
* Replace : Field of variant will replace the parent one.
* Merge : **attributes** will merged from both side.

+ **inventory\_type** must be one of below values and have to be in registered list. If you don't put value in product request, then the latest method will be picked up.

+ **supplier** is an integer constant describe the location of seller 's storage.Each seller can have some **supplier** but each product must be stored in a fixed **supplier**

If seller is in Vietnam, please register your supplier list in TIKI **Seller Center** system

If seller is abroad, you have only one supplier, please contact TIKI supporter to get this value.

**\*\) List inventory\_type :**

| inventory\_type | customer | description |
| :--- | :--- | :--- |
| cross\_border | for Global seller | products is transported from abroad |
| instock | for Vietnamese seller | products in TIKI storage, TIKI pack, TIKI deliver |
| backorder | for Vietnamese seller | products in seller storage, TIKI pack, TIKI deliver |
| seller\_backorder | for Vietnamese seller | products in seller storage, seller pack, seller deliver |
| drop\_ship | for Vietnamese seller | products in seller storage, seller pack, TIKI deliver  |

Example :

```text
{
      "sku": "sku1",
      "quantity": 21,
      "option1": "Black",
      "option2": null,
      "price": 10000001,
      "inventory_type" : "cross_border",
      "supplier" : 239091,
      "image": "https://cdn.fptshop.com.vn/Uploads/Originals/2019/8/8/637008711602926121_SS-note-10-pl-den-1-1.png",
      "images": [
        "https://cdn.fptshop.com.vn/Uploads/Originals/2019/8/8/637008619323404785_SS-note-10-pl-den-2.png",
        "https://cdn.fptshop.com.vn/Uploads/Originals/2019/8/8/637008619327294396_SS-note-10-pl-den-4.png"
      ]
}
```

### 4\) API Docs :

#### 4.1\) Get categories

**GET /{version}/categories**

| description | return the summary list of categories in integration system |
| :--- | :--- |


**4.1.1\) Request**

| Headers | Content-type | application/json |  |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Path Parameters | Name | Type | Mandatory | Example | Description |
| version | String | Y | v1 | version of API |  |
| Query Parameters | Name | Type | Mandatory | Example | Description |
| lang | String | N | en | Filter by language en/vi \( **default is en - english** \) |  |
| name | String | N | book | Filter name by keyword \( case insensitive \) |  |
| parent | Integer | N | 8 | Filter children of this parent\_id only |  |
| primary | Integer | N | 1 | Filter product pushable category |  |

**4.1.2\) Response :** 

Http Status Code: 200

| Field | Type | Description |
| :--- | :--- | :--- |
| root | List&lt;[Category](https://docs.tiki.com.vn/display/BOP/Tiki+Integration+System+Guideline#TikiIntegrationSystemGuideline-1%29Category)&gt; | the summary list of category filtered by request params |

Example : /v1/categories?name=book&primary=1

```text
[
  {
    "id": 218,
    "name": "Medical Books",
    "parent": 320,
    "primary": 1
  },
  {
    "id": 275,
    "name": "Guidebook series",
    "parent": 32,
    "primary": 1
  },
  {
    "id": 2458,
    "name": "Macbook",
    "parent": 8095,
    "primary": 1
  },
  {
    "id": 6225,
    "name": "Bookmark",
    "parent": 18328,
    "primary": 1
  },
  {
    "id": 8780,
    "name": "IELTS Books",
    "parent": 14900,
    "primary": 1
  },
  {
    "id": 8781,
    "name": "TOEIC Books",
    "parent": 14900,
    "primary": 1
  },
  {
    "id": 9295,
    "name": "Picture books",
    "parent": 11018,
    "primary": 1
  }
]
```

**4.1.3\) Exception Case**

| HTTP Code | message | Description |
| :--- | :--- | :--- |
| 500 | Internal server error | having error in server, can't serving |

---------------------------------------------------------------------------------------------------------------

#### 4.2\) Get category detail \( include attribute \)

**GET /{version}/categories/{id}**

| description | retrieve detail of a single categories with its attributes |
| :--- | :--- |


**4.2.1\) Request**

| Headers | Content-type | application/json |  |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Path Parameters | Name | Type | Mandatory | Example | Description |
| version | String | Y | v1 | version of API |  |
| id | Integer | Y | 218 | id of category \( category\_id \) |  |

**4.2.2\) Response**

Http Status Code: 200

| Field | Type | Example | Description |
| :--- | :--- | :--- | :--- |
| id | Integer | 218 | id of category |
| name | String | Children's Books | name of category |
| parent | Integer | 320 | category\_id of parent category |
| primary | Integer | 1 | only primary category can have product |
| description | String | Children's Books \| international purchases buy at \| [Tiki.vn](http://tiki.vn/) \| Cheaper \| Free shipping \| 100% genuine | describe the detail of this category |
| attributes | List&lt;[Attribute](https://docs.tiki.com.vn/display/BOP/Tiki+Integration+System+Guideline#TikiIntegrationSystemGuideline-2%29Attribute)&gt; | see detail below | attributes list of this category |

Example : /v1/categories/218

```text
{
  "id": 218,
  "name": "Children's Books",
  "parent": 320,
  "primary": 1,
  "description": "Children's Books | international purchases buy at | Tiki.vn | Cheaper | Free shipping | 100% genuine",
  "attributes": [
    {
      "id": 498,
      "code": "author",
      "display_name": "Author",
      "is_required": 1
    },
    {
      "id": 1300,
      "code": "book_cover",
      "display_name": "Cover type",
      "is_required": 0
    },
    {
      "id": 799,
      "code": "bulky",
      "display_name": "Is product bulky?",
      "is_required": 0
    }
  ]
}
```

**4.2.3\) Exception Case**

| HTTP Code | message | Description |
| :--- | :--- | :--- |
| 500 | Internal server error | having error in server, can't serving |
| 404 | Category not found |  |

Example :

```text
{
  "errors": [
    "Category not found id: 1"
  ]
}
```

---------------------------------------------------------------------------------------------------------------

#### 4.3\) Create Product

**POST /{version}/products**

| description | create new product |
| :--- | :--- |


**4.3.1\) Request**

| Headers | Content-type | application/json |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| tiki-api | seller token key \( contact Tiki supporter \) |  |  |  |  |  |
| Path Parameters | Name | Type | Mandatory | Example | Description |  |
| version | String | Y | v1 | version of API |  |  |
| Body Parameters | Namespace | Field | Type | Mandatory | Example | Description |
|  | Root | [Product](https://docs.tiki.com.vn/display/BOP/Tiki+Integration+System+Guideline#TikiIntegrationSystemGuideline-3%29Product) | Y | below | product detail to create |  |

**Simple product example :**

You must complete all required attribute from category, all others can be ignored or pass null value

```text
{
  "category_id": 1846,
  "name": "Laptop Dell Vostro 17 XPPP",
  "description": "this is description",
  "market_price": 12222222,
  "attributes": {
    "ram": "8GB",
    "ram_type": "Nvidia",
    "bus": "1333hz",
    "camera": "32MP",
    "audio_technology": "",
    "hard_drive": "1TB",
    "connection_port": "",
    "card_reader": "",
    "brand": "Dell Vostro"
  },
  "image": "https://www.laptopvip.vn/images/laptopvip_cache/e393b8aa5ec204efbea7582fc83a97ad/2/2/2249/original/2990378119/laptop-dell-xps-15-9570-05.png",
  "images": [
    "https://www.laptopvip.vn/images/companies/1/san-pham/untitled%20folder/dell%20xps%209570/800x530xdell-xps-15-2-in-1-review-front-display-1200x9999.jpg",
    "https://www.laptopvip.vn/images/companies/1/san-pham/untitled%20folder/dell%20xps%209570/x39b57caa-31ac-4872-9411-62bc6b15bffd.png"
  ],
  "option_attributes": [],
  "variants": [
    {
      "sku": "sku1",
      "price": 20000,
      "quantity": 20,
      "inventory_type": "cross_border",
      "supplier": 239091
    }
  ]
}
```

**Configurable product example :**

```text
{
  "category_id": 1846,
  "name": "Samsung Galaxy Note 10+",
  "description": "this is description",
  "market_price": 10000000,
  "attributes": {
    "ram": "16GB",
    "ram_type": "Built-in",
    "bus": "1666hz",
    "camera": "",
    "audio_technology": "",
    "hard_drive": "",
    "connection_port": "",
    "brand": "Samsung"
  },
  "image": "http://cdn.fptshop.com.vn/Uploads/Originals/2019/8/9/637009754161317464_samsung-galaxy-note-10-plus-1.jpg",
  "images": [
    "http://cdn.fptshop.com.vn/Uploads/Originals/2019/8/9/637009754161387464_samsung-galaxy-note-10-plus-9.jpg",
    "http://cdn.fptshop.com.vn/Uploads/Originals/2019/8/9/637009754164217464_samsung-galaxy-note-10-plus.jpg"
  ],
  "option_attributes": [
    "color"
  ],
  "variants": [
    {
      "sku": "sku1",
      "quantity": 21,
      "option1": "Black",
      "option2": null,	
      "price": 10000001,
      "inventory_type": "cross_border",
      "supplier": 239091,
      "image": "https://cdn.fptshop.com.vn/Uploads/Originals/2019/8/8/637008711602926121_SS-note-10-pl-den-1-1.png",
      "images": [
        "https://cdn.fptshop.com.vn/Uploads/Originals/2019/8/8/637008619323404785_SS-note-10-pl-den-2.png",
        "https://cdn.fptshop.com.vn/Uploads/Originals/2019/8/8/637008619327294396_SS-note-10-pl-den-4.png"
      ]
    },
    {
      "sku": "sku2",
      "quantity": 22,
      "option1": "White",
      "price": 10000002,
      "inventory_type": "cross_border",
      "supplier": 239091,
      "image": "https://cdn.fptshop.com.vn/Uploads/Originals/2019/8/8/637008710284136121_SS-note-10-pl-trang-1-1.png",
      "images": [
        "https://cdn.fptshop.com.vn/Uploads/Originals/2019/8/8/637008624715439429_SS-note-10-pl-trang-2.png",
        "https://cdn.fptshop.com.vn/Uploads/Originals/2019/8/8/637008624715279445_SS-note-10-pl-trang-3.png"
      ]
    }
  ]
}
```

**\*Note**: To understand the relation between variant and it's product parent please read the detail from : [Variant](https://docs.tiki.com.vn/display/BOP/Tiki+Integration+System+Guideline#TikiIntegrationSystemGuideline-4%29Variant)

**4.3.2\) Response**

Http Status Code: 200

| Field | Type | Example | Description |
| :--- | :--- | :--- | :--- |
| trace\_id | String | `c3587ec50976497f837461e0c2ea3da5` | trace\_id to tracking this request |
| state | String | queuing | current state of your request |

Example : 

```text
{
    "trace_id": "c3587ec50976497f837461e0c2ea3da5",
    "state": "queuing"
}
```

**4.3.3\) Exception Case:**

| HTTP Code | message | Description |
| :--- | :--- | :--- |
| 500 | Internal server error | having error in server, can't serving |
| 400 | Bad request | missing header or required params |
| 401 | Unauthorized | your tiki-api token is not valid |
| 404 | Not found | Attributes not found for category\_id in payload |
| 422 | Unprocessable Entity | Payload is missing or invalid field  |
| 429 | Too Many Requests | Your rate limit is exceed |

Configurable Product have invalid payload  \( missing option2 value & price in sku2 \)

```text
{
  "category_id": 1846,
  "name": "Samsung Galaxy Note 10+",
  "description": "this is description",
  "market_price": 10000000,
  "attributes": {
    "ram": "16GB",
    "ram_type": "Built-in",
    "bus": "1666hz",
    "camera": "",
    "audio_technology": "",
    "hard_drive": "",
    "connection_port": "",
    "brand": "Samsung"
  },
  "image": "http://cdn.fptshop.com.vn/Uploads/Originals/2019/8/9/637009754161317464_samsung-galaxy-note-10-plus-1.jpg",
  "images": [
    "http://cdn.fptshop.com.vn/Uploads/Originals/2019/8/9/637009754161387464_samsung-galaxy-note-10-plus-9.jpg",
    "http://cdn.fptshop.com.vn/Uploads/Originals/2019/8/9/637009754164217464_samsung-galaxy-note-10-plus.jpg"
  ],
  "option_attributes": [
    "color",
    "storage"
  ],
  "variants": [
    {
      "sku": "sku1",
      "quantity": 21,
      "option1": "Black",
      "option2": "32GB",
      "price": 10000001,
      "inventory_type": "cross_border",
      "supplier": 239091,
      "image": "https://cdn.fptshop.com.vn/Uploads/Originals/2019/8/8/637008711602926121_SS-note-10-pl-den-1-1.png",
      "images": [
        "https://cdn.fptshop.com.vn/Uploads/Originals/2019/8/8/637008619323404785_SS-note-10-pl-den-2.png",
        "https://cdn.fptshop.com.vn/Uploads/Originals/2019/8/8/637008619327294396_SS-note-10-pl-den-4.png"
      ]
    },
    {
      "sku": "sku2",
      "quantity": 22,
      "option1": "White",
      "inventory_type": "cross_border",
      "supplier": 239091,
      "image": "https://cdn.fptshop.com.vn/Uploads/Originals/2019/8/8/637008710284136121_SS-note-10-pl-trang-1-1.png",
      "images": [
        "https://cdn.fptshop.com.vn/Uploads/Originals/2019/8/8/637008624715439429_SS-note-10-pl-trang-2.png",
        "https://cdn.fptshop.com.vn/Uploads/Originals/2019/8/8/637008624715279445_SS-note-10-pl-trang-3.png"
      ]
    }
  ]
}
```

Response: 

```text
{
    "errors": [
        "variants don't match with option_attributes => variants option2 missing",
        "variants should have price > 0"
    ]
}
```

---------------------------------------------------------------------------------------------------------------

#### 4.4\) Tracking history

**GET /{version}/tracking** 

| description | tracking latest request of user \( via token \) |
| :--- | :--- |


**4.4.1\) Request**

| Headers | Content-type | application/json |  |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- |
|  | tiki-api | seller token key \( contact Tiki supporter \) |  |  |  |
| Path Parameters | Name | Type | Mandatory | Example | Description |
| version | String | Y | v1 | version of API |  |
| Query Parameters | Name | Type | Mandatory | Example | Description |
| limit | Integer | N | 50 | return up to this many requests |  |
| created\_at\_min | String | N | 2019-06-27 10:47:34 | Show request created after date \( format as example \)  |  |
| created\_at\_max | String | N | 2019-06-27 10:47:34 | Show request created before date \( format as example \)  |  |
| updated\_at\_min | String | N | 2019-06-27 10:47:34 | Show request updated after date \( format as example \)  |  |
| updated\_at\_max | String | N | 2019-06-27 10:47:34 | Show request created before date \( format as example \)  |  |

**4.4.2\) Response**

Http Status Code: 200

| Field | Type | Description |
| :--- | :--- | :--- |
| root | List&lt;Request&gt; | list filtered request match with request params |

**Entity** : Request

| Field | Type | Example | Description | Note |
| :--- | :--- | :--- | :--- | :--- |
| trace\_id | String | `c3587ec50976497f837461e0c2ea3da5` | trace\_id to tracking this request |  |
| state | String | processing | current state of your request |  |
| reason | String | Image does not match product name | the reason why your request is rejected | only rejected request have reason |
| tiki\_sku | String | 2150725160607 | TIKI sku when product created successfully | only approved request have tiki\_sku |

Example : 

```text
[
  {
    "trace_id": "c3587ec50976497f837461e0c2ea3da5",
    "state": "processing",
    "reason": null,
    "tiki_sku": null
  },
  {
    "trace_id": "c3587ec50976497f83edfgsdfgsdfgf5",
    "state": "rejected",
    "reason": "Image does not match product name",
    "tiki_sku": null
  },
  {
    "trace_id": "c3587ec50976497f837463gfsdfgbsfg",
    "state": "approved",
    "reason": null,
    "tiki_sku": "2150725160607"
  }
]
```

**4.4.3\) State list**

| State | Description |
| :--- | :--- |
| queuing | request is in queue, waiting for processing |
| processing | request is transforming to tiki format  |
| **drafted** | TIKI product request created, ready to review |
| bot\_awaiting\_approve | TIKI reviewing request by bot |
| md\_awaiting\_approve | TIKI reviewing required document \( by category \) |
| awaiting\_approve | request waiting for approving, we need to take a look |
| **approved** | request is approved, product created successfully |
| **rejected** | request is rejected, use tracking API for more information |
| deleted | request is deleted, no more available in system |

**4.4.4\) Exception Case**

| HTTP Code | message | Description |
| :--- | :--- | :--- |
| 500 | Internal server error | having error in server, can't serving |
| 400 | Bad request | missing header or required params |
| 401 | Unauthorized | your tiki-api token is not valid |
| 429 | Too Many Requests | Your rate limit is exceed |

---------------------------------------------------------------------------------------------------------------

#### 4.5\) Tracking a request

**GET /{version}/tracking/{trace\_id}** 

| description | retrieve detail of a single request |
| :--- | :--- |


**4.5.1\) Request**

| Headers | Content-type | application/json |  |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- |
|  | tiki-api | seller token key \( contact Tiki supporter \) |  |  |  |
| Path Parameters | Name | Type | Mandatory | Example | Description |
| version | String | Y | v1 | version of API |  |
| trace\_id | String | Y | `c3587ec50976497f837461e0c2ea3da5` | trace\_id of request get from product API |  |

**4.5.2\) Response**

Http Status Code: 200

| Field | Type | Example | Description | Note |
| :--- | :--- | :--- | :--- | :--- |
| trace\_id | String | `c3587ec50976497f837461e0c2ea3da5` | trace\_id to tracking this request |  |
| state | String | processing | current state of your request |  |
| reason | String | Image does not match product name | the reason why your request is rejected | only rejected request have reason |
| tiki\_sku | String | 2150725160607 | TIKI sku when product created successfully | only approved request have tiki\_sku |

Example : 

```text
{
  "trace_id": "c3587ec50976497f837461e0c2ea3da5",
  "state": "rejected",
  "reason": "Image does not match product name",
  "tiki_sku": null
}
```

**4.5.3\) State list**

| State | Description |
| :--- | :--- |
| queuing | request is in queue, waiting for processing |
| processing | request is transforming to tiki format  |
| **drafted** | TIKI product request created, ready to review |
| bot\_awaiting\_approve | TIKI reviewing request by bot |
| md\_awaiting\_approve | TIKI reviewing required document \( by category \) |
| awaiting\_approve | request waiting for approving, we need to take a look |
| **approved** | request is approved, product created successfully |
| **rejected** | request is rejected, use tracking API for more information |
| deleted | request is deleted, no more available in system |

**4.5.4\) Exception Case**

| HTTP Code | message | Description |
| :--- | :--- | :--- |
| 500 | Internal server error | having error in server, can't serving |
| 400 | Bad request | missing header or required params |
| 401 | Unauthorized | your tiki-api token is not valid |
| 404 | TraceId not found | traceId is invalid |
| 429 | Too Many Requests | Your rate limit is exceed |

---------------------------------------------------------------------------------------------------------------

#### 4.6\) Update variant price/quantity/active

**POST /{version}/products/{sku}/updateSku**

| description | update non validate fields like price/quantity/active of a created product |
| :--- | :--- |


**4.6.1\) Request**

| Headers | Content-type | application/json |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| tiki-api | seller token key \( contact Tiki supporter \) |  |  |  |  |  |
| Path Parameters | Name | Type | Mandatory | Example | Description |  |
| version | String | Y | v1 | version of API |  |  |
| sku | String | Y | DANG19951995 | the original sku from your side system |  |  |
| Body Parameters | Namespace | Field | Type | Mandatory | Example | Description |
| Variant | price | [I](https://docs.tiki.com.vn/display/BOP/Tiki+Integration+System+Guideline#TikiIntegrationSystemGuideline-3%29Product)nteger | N | 10000 | product 's new price |  |
|  | quantity | String | N | 10 | product 's new quantity |  |
|  | active | Integer | N | 1 | product 's new status \( 1=active / 0=inactive\) |  |

Example: /v1/products/DANG19951995/updateSku

```text
{
  "price": 100000,
  "quantity": 20,
  "active": 1
}
```

**4.6.2\) Response**

Http Status Code: 200

| Field | Type | Example | Description |
| :--- | :--- | :--- | :--- |
| state | String | approved | your product is updated successfully |

Example : 

**4.6.3\) Exception Case**

| HTTP Code | message | Description |
| :--- | :--- | :--- |
| 500 | Internal server error | having error in server, can't serving |
| 400 | Bad request | your request is invalid |
| 401 | Unauthorized | your tiki-api token is not valid |
| 403 | Forbidden | your tiki-api token do not have permission to perform |
| 422 | Unprocessable Entity | your request body have invalid value |
| 429 | Too Many Requests | Your rate limit is exceed |

Example :

```text
{
    "errors": [
        "Price is not valid because less than ten percent listing price"
    ]
}
```

## VI \) Order Endpoint 

### 1\) Let's get started

Whenever customer place an order, TIKI and seller have to collaborate to delivery the product to customer as soon as possible

TIKI have 2 main fulfillment type chosen when seller create product. Each type have a different flow to confirm order and deliver product to customer.

* TIKI delivery \( **tiki\_delivery** \) 
* Seller delivery \( **seller\_delivery** \)

Via API , we provide some solution to confirm order & delivery status step by step :

* Query list registered warehouse , specific order 
* Confirm product available status \( select warehouse it belong to \)
* Confirm delivery status \( for seller delivery \) 
* Print order label \( if needed \) 

Please take look those API docs below for more detail 

### 2 \) Sequence diagram

#### 2.1\) Seller delivery

**\(1\)** Seller get warehouses using api \***get warehouses,** each warehouse have location, _warehouse\_code_ and _seller\_delivery\_id_

**\(2\)** Seller pull order, using api \***get list order**  with status = "_**queueing**_"

**\(3\)** After pull order, seller will confirm each item in the list, for-each item in orders, seller confirm one _seller\_inventory\_id_ have item in stock. Using api \***confirm order items**

**\(4\)** After seller delivery, seller will confirm delivery status using api \***Confirm delivery status,** status=canceled or status=successful\_delivery. If you canceled order then using field reason \(reason cancel order\).

#### 2.2\) TIKI Delivery

**\(1\)** Seller get warehouses using api \***get warehouses,** each warehouse have location, _warehouse\_code_ and _seller\_delivery\_id_

**\(2\)** Seller pull order, using api \***get list order**  with status = "_**queueing**_"

**\(3\)**  After pull order, seller will confirm each item in the list, for-each item in orders, seller confirm one _seller\_inventory\_id_ have item in stock. Using api \***confirm order items**

**\(4\)** Once products in TIKI warehouse, we will pack and delivery to customer.

**\(5\)** Finally TIKI will confirm delivery order using api \***Confirm delivery,** status=canceled or status=successful\_delivery. 

### 3 \) Entity 

#### 3.1\) Order

<table>
  <thead>
    <tr>
      <th style="text-align:left">Field</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Example</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">order_code</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&quot;239545064&quot;</td>
      <td style="text-align:left">An unique key that auto generated by system</td>
    </tr>
    <tr>
      <td style="text-align:left">coupon_code</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&quot;123&quot;</td>
      <td style="text-align:left">A coupon code apply for order.</td>
    </tr>
    <tr>
      <td style="text-align:left">status</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&quot;complete&quot;</td>
      <td style="text-align:left">Order status, see List of Order Status for more details</td>
    </tr>
    <tr>
      <td style="text-align:left">total_price_before_discount</td>
      <td style="text-align:left">Long</td>
      <td style="text-align:left">598000</td>
      <td style="text-align:left">Total order amount before discounts</td>
    </tr>
    <tr>
      <td style="text-align:left">total_price_after_discount</td>
      <td style="text-align:left">Long</td>
      <td style="text-align:left">528000</td>
      <td style="text-align:left">Total order amount after applied discounts</td>
    </tr>
    <tr>
      <td style="text-align:left">updated_at</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&quot;2019-07-02 18:56:29&quot;</td>
      <td style="text-align:left">Date-time when the order was updated</td>
    </tr>
    <tr>
      <td style="text-align:left">purchased_at</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&quot;2019-07-02 18:56:29&quot;</td>
      <td style="text-align:left">Date-time when the order was placed</td>
    </tr>
    <tr>
      <td style="text-align:left">fulfillment_type</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&quot;cross_border&quot;</td>
      <td style="text-align:left">Order fulfillment types:
        <br />- Seller Delivery orders are fulfillment_type = <b>seller_delivery</b>
        <br
        />- Orders from abroad (Crossborder) are orders with fulfillment_type = <b>cross_border</b>
        <br
        />- Dropship orders directly from the seller (Dropship) are orders with
        fulfillment_type = <b>dropship</b>
        <br />- Tiki Delivery (Tiki Delivery) are orders with fulfillment_type = <b>tiki_delivery</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">note</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&quot;&quot;</td>
      <td style="text-align:left">delivery note</td>
    </tr>
    <tr>
      <td style="text-align:left">is_rma</td>
      <td style="text-align:left">Integer</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">
        <p>default 0,</p>
        <p>1: is return order</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">warehouse_id</td>
      <td style="text-align:left">Integer</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">id of warehouse</td>
    </tr>
    <tr>
      <td style="text-align:left">handling_fee</td>
      <td style="text-align:left">Long</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">handling fee</td>
    </tr>
    <tr>
      <td style="text-align:left">collectable_total_price</td>
      <td style="text-align:left">Long</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">amount to be collected from customer</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>discount</b>(*)</td>
      <td style="text-align:left">Object</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">discount info</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>tax</b>(*)</td>
      <td style="text-align:left">Object</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">tax invoice info</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>shipping</b>(*)</td>
      <td style="text-align:left">Object</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">shipping info</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>items</b>(*)</td>
      <td style="text-align:left">Array Object</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">list of item</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>payment</b>(*)</td>
      <td style="text-align:left">Object</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">payment info</td>
    </tr>
  </tbody>
</table>#### 3.2\) Discount 

| Field | Type | Example | Description |
| :--- | :--- | :--- | :--- |
| discount\_amount | Long | 10000 | total amount discount |
| discount\_coupon | Long | 10000 | amount discount of coupon |

#### 3.3\) Tax

| Field | Type | Example | Description |
| :--- | :--- | :--- | :--- |
| code | String | 1 |  |
| name | String | Company |  |
| address | String | Ha Noi |  |

#### 3.4\) Shipping

| Field | Type | Example | Description |
| :--- | :--- | :--- | :--- |
| name | String | "hoàng anh" | Name of order receiver |
| street | String | "58/13  hậu giang" | Street address for order delivery |
| ward | String | "Phường 06" | The ward of delivery address |
| city | String | "Quận 6" | The district of delivery address |
| region | String | "Hồ Chí Minh" | Province, City of delivery address |
| country | String | "VN" | The country of delivery address |
| phone | String | "0784083498" | phone number |
| estimation\_description | String | Expected delivery on Friday | the delivery info TIKI estimate  |
| shipping\_fee | Long | 0 | shipping fee |

#### 3.5\) Item \( Order item \)

| Field | Type | Example | Description |
| :--- | :--- | :--- | :--- |
| id | Integer | 90965636 | Identifier of order line item within Tiki seller center |
| product\_id | Integer | 12033331 | Product ID within Tiki seller center |
| product\_name | String | "Gối Massage Cổ Hình Chữ U Xiaomi LR-S100" | Product name |
| sku | String | 341231234 | Identifier of the product within third-party system, that is added as a field of the product in Tiki system. |
| original\_sku | String | "H24071" | Code of the seller SKU within Tiki Seller Center. |
| qty | Integer | 2 | Item quantity in the order |
| price | Integer | 299000 | Sales price of the product in the order |
| confirmation\_status | String | "confirmed" | Confirmation status of the order item  |
| confirmed\_at | String | "2019-07-03 09:18:08" | The time when the order item was confirmed |
| must\_confirmed\_before\_at | String | "2019-07-03 09:18:08" | Order must confirm before at that time |
| **inventory\_type**\(\*\) | String | instock | is product inventory type |

#### 3.6\) Payment

| Field | Type | Example | Description |
| :--- | :--- | :--- | :--- |
| payment\_method | String | "cod" | the payment method of customer |
| updated\_at | String | "2019-07-31 14:31:26" | the latest time payment info is updated |
| description | String | "Thanh toán tiền mặt khi nhận hàng" | the detail of payment |

#### 3.7\) Order status

| **Value** | **Value-VN** | **Description** | **Description - VN** |
| :--- | :--- | :--- | :--- |
| queueing | cho\_in | tiki received | Chờ in |
| canceled | canceled | canceled order | Đã hủy |
| successful\_delivery | giao\_hang\_thanh\_cong | successful delivery | Giao hàng thành công |
| complete |  |  |  |

### 4\) API Docs

#### 4.1\) Get list orders

URI: GET /{version}/orders

| description | Returns a list of sales orders managed by signing in seller, base on a specific search query. |
| :--- | :--- |


**4.1.1\) Request**

<table>
  <thead>
    <tr>
      <th style="text-align:left">Headers</th>
      <th style="text-align:left">Content-type</th>
      <th style="text-align:left">application/json</th>
      <th style="text-align:left"></th>
      <th style="text-align:left"></th>
      <th style="text-align:left"></th>
      <th style="text-align:left"></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">tiki-api</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">Query Parameters</td>
      <td style="text-align:left">Name</td>
      <td style="text-align:left">Type</td>
      <td style="text-align:left">Mandatory</td>
      <td style="text-align:left">Example</td>
      <td style="text-align:left">Default value</td>
      <td style="text-align:left">Description</td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">status</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">N</td>
      <td style="text-align:left">&quot;queueing&quot;</td>
      <td style="text-align:left">queuing</td>
      <td style="text-align:left">
        <p>Order status:</p>
        <ul>
          <li>queueing</li>
          <li>seller_confirmed</li>
          <li>seller_canceled</li>
          <li>complete</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">page</td>
      <td style="text-align:left">Integer</td>
      <td style="text-align:left">N</td>
      <td style="text-align:left">2</td>
      <td style="text-align:left">1</td>
      <td style="text-align:left">Page no of paging</td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">limit</td>
      <td style="text-align:left">Integer</td>
      <td style="text-align:left">N</td>
      <td style="text-align:left">20</td>
      <td style="text-align:left">20</td>
      <td style="text-align:left">Number of records per page</td>
    </tr>
  </tbody>
</table>\*Note : We support query data 30 days latest at most.

**4.1.2\) Response :** 

Http Status Code: 200

| Field | Type | Description |
| :--- | :--- | :--- |
| results | List&lt;Order&gt; | Returns a list of sales orders managed by signing in seller, base on a specific search query. |
| paging | Object | Paging information |

Example : /v1/orders?page=1&limit=2&status=queueing  
**Response**

```text
{
  "data": [
    {
      "order_code": "929231617",
      "coupon_code": "123",
      "status": "queueing",
      "total_price_before_discount": 200000,
      "total_price_after_discount": 200000,
      "updated_at": "2019-10-30 17:27:24",
      "purchased_at": "2019-10-30 17:27:17",
      "fulfillment_type": "dropship",
      "note": "",
      "is_rma": null,
      "warehouse_id": 17,
      "tax": {
        "code": null,
        "name": null,
        "address": null
      },
      "discount": {
        "discount_amount": 10,
        "discount_coupon": 10
      },
      "shipping": {
        "name": "hậu nguyễn",
        "street": "519 kim mã",
        "ward": "Phường Kim Mã",
        "city": "Quận Ba Đình",
        "region": "Hà Nội",
        "country": "VN",
        "phone": "0912611089",
        "estimate_description": "Dự kiến giao hàng vào Thứ hai, 04/11/2019"
      },
      "items": [
        {
          "id": 25203463,
          "product_id": 2050232,
          "product_name": "Cơm gà - dropship -hn4",
          "sku": "9956112228645",
          "original_sku": "",
          "qty": 1,
          "price": 200000,
          "confirmation_status": "waiting",
          "confirmed_at": "",
          "must_confirmed_before_at": "2019-10-31 12:00:00",
          "inventory_type": "instock"
        }
      ],
      "payment": {
        "payment_method": "cod",
        "updated_at": "2019-10-30 17:27:25",
        "description": "Thanh toán tiền mặt khi nhận hàng"
      },
      "handling_fee": 0,
      "collectable_total_price": 0
    }
  ],
  "paging": {
    "total": 1,
    "per_page": 20,
    "current_page": 1,
    "last_page": 1,
    "from": 1,
    "to": 1
  }
}
```

| HTTP Code | message | Description |
| :--- | :--- | :--- |
| 500 | Internal server error | having error in server, can't serving |

**4.1.3\)Exception Case**

#### 4.2\) Order detail

URI: `GET` /{version}/orders/{order\_code}

| description | Returns detail information including product items of a sales order, base on order code. |
| :--- | :--- |


**4.2.1\) Request**

| Headers | Content-type | application/json |
| :--- | :--- | :--- |
| tiki-api | String | seller token key \( contact Tiki supporter \)  |

**4.2.2\) Response :** 

Http Status Code: 200

| Field | Type | Description |
| :--- | :--- | :--- |
| root | Order | Returns detail information including product items of a sales order, base on order code. |

Example : /v1/orders/101539588  
**Response**

```text
{
    "order_code": "929231617",
    "coupon_code": null,
    "status": "queueing",
    "total_price_before_discount": 200000,
    "total_price_after_discount": 200000,
    "updated_at": "2019-10-30 17:27:24",
    "purchased_at": "2019-10-30 17:27:17",
    "fulfillment_type": "dropship",
    "note": "",
    "is_rma": null,
    "warehouse_id": 17,
    "discount": {
        "discount_amount": 0,
        "discount_coupon": 0
    },
    "tax": {
        "code": null,
        "name": null,
        "address": null
    },
    "shipping": {
        "name": "hậu nguyễn",
        "street": "519 kim mã",
        "ward": "Phường Kim Mã",
        "city": "Quận Ba Đình",
        "region": "Hà Nội",
        "country": "VN",
        "phone": "0912611089",
        "estimate_description": "Dự kiến giao hàng vào Thứ hai, 04/11/2019",
        "shipping_fee": 0
    },
    "items": [
        {
            "id": 25203463,
            "product_id": 2050232,
            "product_name": "Cơm gà - dropship -hn4",
            "sku": "9956112228645",
            "original_sku": "",
            "qty": 1,
            "price": 200000,
            "confirmation_status": "seller_confirmed",
            "confirmed_at": "2019-11-01 10:07:58",
            "must_confirmed_before_at": "2019-11-01 23:59:59",
            "inventory_type": "seller_backorder"
        }
    ],
    "payment": {
        "payment_method": "cod",
        "updated_at": "2019-10-30 17:27:25",
        "description": "Thanh toán tiền mặt khi nhận hàng"
    },
    "handling_fee": 0,
    "collectable_total_price": 0
}
```

**4.2.3\) Exception Case**

| HTTP Code | message | Description |
| :--- | :--- | :--- |
| 500 | Internal server error | having error in server, can't serving |
| 404 | Not found | order code not found |

#### 4.3\) Get warehouses

URI: GET /{version}/warehouses

| description | Returns detail information of warehouse of Tiki that seller registries for backorder model. |
| :--- | :--- |


**4.3.1\) Request**

| Headers | Content-type | application/json |  |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- |
|  | tiki-api | seller token key \( contact Tiki supporter \)  |  |  |  |
| Query Parameters | Name | Type | Mandatory | Example | Description |
|  | warehouse\_id | Integer | N | 2 | the id of warehouse |
|  | warehouse\_code | String | N | hn | the code of warehouse |

**4.3.2\) Response**

Http Status Code: 200

| Field | Type | Description |
| :--- | :--- | :--- |
| root | Array object | List of warehouse object |

Example[ /{version}/warehouses](http://docs.tiki.com.vn/integration/backorder/warehouses)  
Response

```text
[
  {
    "contact_email": "dsada@gmail.com",
    "contact_name": "Nguyễn Thị Bích a",
    "contact_phone": "0988909999",
    "country": {
      "code": "vn",
      "name": "Viet Nam"
    },
    "district": {
      "code": "VN034025",
      "name": "Quận Hoàng Mai"
    },
    "name": "Kho giáp bát hà nội",
    "region": {
      "code": "VN034",
      "name": "Hà Nội"
    },
    "seller_inventory_id": 919,
    "street": "dsa ds",
    "ward": {
      "code": "VN034025003",
      "name": "Phường Giáp Bát"
    },
    "warehouse_code": "hn",
    "warehouse_id": 2
  },
  {
    "contact_email": "32@gmail.com",
    "contact_name": "7",
    "contact_phone": "0887999123",
    "country": {
      "code": "vn",
      "name": "Viet Nam"
    },
    "district": {
      "code": "VN023007",
      "name": "Quận Ninh Kiều"
    },
    "name": "7",
    "region": {
      "code": "VN023",
      "name": "Cần Thơ"
    },
    "seller_inventory_id": 905,
    "street": "3432",
    "ward": {
      "code": "VN023007005",
      "name": "Phường An Khánh"
    },
    "warehouse_code": "ct",
    "warehouse_id": 6
  },
  {
    "contact_email": "dsdss@gmail.com",
    "contact_name": "6",
    "contact_phone": "0912676787",
    "country": {
      "code": "vn",
      "name": "Viet Nam"
    },
    "district": {
      "code": "VN037011",
      "name": "Quận Hồng Bàng"
    },
    "name": "kho 6",
    "region": {
      "code": "VN037",
      "name": "Hải Phòng"
    },
    "seller_inventory_id": 904,
    "street": "dsdsd",
    "ward": {
      "code": "VN037011003",
      "name": "Phường Hùng Vương"
    },
    "warehouse_code": "hp",
    "warehouse_id": 8
  },
  {
    "contact_email": "email@gmail.com",
    "contact_name": "kho 5",
    "contact_phone": "0989000912",
    "country": {
      "code": "vn",
      "name": "Viet Nam"
    },
    "district": {
      "code": "VN039013",
      "name": "Quận 5"
    },
    "name": "kho 5",
    "region": {
      "code": "VN039",
      "name": "Hồ Chí Minh"
    },
    "seller_inventory_id": 903,
    "street": "ewew",
    "ward": {
      "code": "VN039013007",
      "name": "Phường 07"
    },
    "warehouse_code": "sgn",
    "warehouse_id": 4
  },
  {
    "contact_email": "dsf@gmail.com",
    "contact_name": "1",
    "contact_phone": "0912345666",
    "country": {
      "code": "vn",
      "name": "Viet Nam"
    },
    "district": {
      "code": "VN034019",
      "name": "Quận Bắc Từ Liêm"
    },
    "name": "kho hà nội",
    "region": {
      "code": "VN034",
      "name": "Hà Nội"
    },
    "seller_inventory_id": 902,
    "street": "ewqeqw",
    "ward": {
      "code": "VN034019006",
      "name": "Phường Minh Khai"
    },
    "warehouse_code": "hn",
    "warehouse_id": 2
  },
  {
    "contact_email": "ntbh@gmail.com",
    "contact_name": "hoa",
    "contact_phone": "0989787878",
    "country": {
      "code": "vn",
      "name": "Viet Nam"
    },
    "district": {
      "code": "VN034022",
      "name": "Quận Hà Đông"
    },
    "name": "Kho 2",
    "region": {
      "code": "VN034",
      "name": "Hà Nội"
    },
    "seller_inventory_id": 899,
    "street": "hà đông",
    "ward": {
      "code": "VN034022003",
      "name": "Phường Dương Nội"
    },
    "warehouse_code": "hn",
    "warehouse_id": 2
  }
]
```

**4.3.3\)Exception Case**

| HTTP Code | message | Description |
| :--- | :--- | :--- |
| 500 | Internal server error | having error in server, can't serving |

#### 4.4\) Confirm order items

URI: `POST` /{version}/orders/confirmItems

| description | Seller confirm available status and location of each item in the list |
| :--- | :--- |


**4.4.1\) Request**

| Headers | Content-type | application/json |  |  |
| :--- | :--- | :--- | :--- | :--- |
|  | tiki-api | seller token key \( contact Tiki supporter \)  |  |  |
| Body Parameters | Name | Type | Mandatory | Description |
|  | order\_code | String | Y | order code |
|  | item\_ids | Array Integer | Y | list of item\_id of a specific backorder that seller want to confirm,  |
|  | warehouse\_code | String | Y | warehouse code |
|  | seller\_inventory\_id | String | Y | seller inventory id |
|  | delivery\_commitment\_time\(\*\) | String | N | Seller delivery commitment time, String datetime with format Y-m-d H:i:s. |
|  | tracking\_number\(\*\) | String | N | maybe equal order code, tracking\_number is code for tracking order via 3rd party system or anything like this  |

**\*Note:** We use this endpoint to confirm available item only, if an item is absent, it will be confirmed as not able to sell by this time.

So if you want to reject all of item in this order, just send an empty **item\_ids** list

* **delivery\_commitment\_time** is required for **seller delivery** order
* **tracking\_number** is required for **cross\_border** order

**4.4.2\) Response :** 

Http Status Code: 200

| Field | Type | Description |
| :--- | :--- | :--- |
| code | String | Seller confirm order |
| data | Array object |  |

Example:

* Seller delivery

```text
{
  "order_code": "419060832",
  "warehouse_code": "sg",
  "seller_inventory_id": 882,
  "item_ids": [94792486, 94792487],
  "delivery_commitment_time": "2019-11-03 23:59:59"  
}
```

* TIKI delivery

```text
{
  "order_code": "419060832",
  "warehouse_code": "sg",
  "seller_inventory_id": 882,
  "item_ids": [94792486, 94792487]
}
```

**Response**

| `{`   `"code": 200,`   `"data": []` `}` |
| :--- |


**4.4.3\) Exception Case**

| HTTP Code | message | Description |
| :--- | :--- | :--- |
| 500 | Internal server error | having error in server, can't serving |
| 400 | Bad request | Params in body request invalid. See detail response |

#### 4.5\) Confirm delivery

URI: POST /{version}/orders/confirmDelivery

| description | Confirm delivery for **order of seller delivery**. Update status complete\|canceled delivery order, same as Seller center. |
| :--- | :--- |


**4.5.1\)  Request**

<table>
  <thead>
    <tr>
      <th style="text-align:left">Headers</th>
      <th style="text-align:left">Content-type</th>
      <th style="text-align:left">application/json</th>
      <th style="text-align:left"></th>
      <th style="text-align:left"></th>
      <th style="text-align:left"></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">tiki-api</td>
      <td style="text-align:left">seller token key ( contact Tiki supporter )</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">Body Parameters</td>
      <td style="text-align:left">Name</td>
      <td style="text-align:left">Type</td>
      <td style="text-align:left">Mandatory</td>
      <td style="text-align:left">Description</td>
      <td style="text-align:left">Example</td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">order_code</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">Y</td>
      <td style="text-align:left">Order code of seller delivery.</td>
      <td style="text-align:left">&quot;20939384&quot;</td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">status</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">Y</td>
      <td style="text-align:left">
        <p>Status of delivery, have options:
          <br />
        </p>
        <ul>
          <li>canceled: Delivery canceled.</li>
          <li>successful_delivery: Delivery successful.</li>
        </ul>
      </td>
      <td style="text-align:left">&quot;successful_delivery&quot;</td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">reason (*)</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">N</td>
      <td style="text-align:left">Reason cancel order options: (using when cancel order)</td>
      <td style="text-align:left">&quot;Could not be reached&quot;</td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">appointment_date</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">N</td>
      <td style="text-align:left">String datetime with format Y-m-d H:i:s.</td>
      <td style="text-align:left">&quot;2019-06-22 18:12:17&quot;</td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">document_urls</td>
      <td style="text-align:left">Array urls</td>
      <td style="text-align:left">N</td>
      <td style="text-align:left">Array urls of handling documents (ex: screen soft), max 3.</td>
      <td style="text-align:left">[]</td>
    </tr>
  </tbody>
</table>\(\*\)Reason should be selected in this list, else it will be chosen as **others** so you can fill any reason in free text

| reason | Description | Description-vn |
| :--- | :--- | :--- |
| _seller-out-of-stock-cancel-order_  | seller out of stock, cancel order | Nhà bán hết hàng - hủy đơn hàng. |
| _customers-are-no-longer-in-demand_ | Customers are no longer in demand | Khách hàng không còn nhu cầu. |
| -free text here- | any reason if you can't select from list | Một lý do nào đó |

**4.5.2\) Response**

Http Status Code: 200

| Field | Type | Example | Description |
| :--- | :--- | :--- | :--- |
| order\_code | String | "20939384" | Order code |

Example :   
**Body**

| `{`     `"order_code": "20939384",`     `"status": "canceled",`     `"reason": "`_`customers-are-no-longer-in-demand`_`"` `}` |
| :--- |


  
**Response**

| `{`   `"order_code": "20939384"` `}` |
| :--- |


**4.5.3\) Exception Case**

| HTTP Code | message | Description |
| :--- | :--- | :--- |
| 500 | Internal server error | having error in server, can't serving |
| 400 | Bad request | request not valid |

#### 4.6\) Update delivery status

URI: POST /{version}/orders/updateDeliveryStatus

| description | Update delivery status, base on order codes. When order delivery, we need know order delivery status, you will need update it. |
| :--- | :--- |


**4.6.1\)  Request**

| Headers | Content-type | application/json |  |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- |
|  | tiki-api | seller token key \( contact Tiki supporter \)  |  |  |  |
| Body Parameters | Name | Type | Mandatory | Description | Example |
|  | order\_code | String | Y | Order code of seller delivery. | "20939384" |
|  | status | String | Y | Status of delivery. | "ready for delivery" |
|  | update\_time | String | Y | String datetime with format Y-m-d H:i:s. | "2019-06-22 18:12:17" |
|  | payload | String object | N | String object of your event payload. |  |

**4.6.2\) Response**

Http Status Code: 200

| Field | Type | Example | Description |
| :--- | :--- | :--- | :--- |
| message | String | "success" |  |

Example :   
**Body**

| `{`       "order\_code": "327965376-5vHl1b",       "payload": "",       "update\_time":"2019-11-23 23:59:59",       "status": "ready for delivery" `}` |
| :--- |


  
**Response**

| `{`   `"message": "success"` `}` |
| :--- |


**4.6.3\) Exception Case**

| HTTP Code | message | Description |
| :--- | :--- | :--- |
| 500 | Internal server error | having error in server, can't serving |
| 400 | Bad request | request not valid |

#### 4.7\) Print order labels

URI: GET /{version}/orders/{order\_code}/print

| description | Return shipping or invoice label url of sale orders, base on order codes. |
| :--- | :--- |


**4.7.1\) Request**

<table>
  <thead>
    <tr>
      <th style="text-align:left">Headers</th>
      <th style="text-align:left">Content-type</th>
      <th style="text-align:left">application/json</th>
      <th style="text-align:left"></th>
      <th style="text-align:left"></th>
      <th style="text-align:left"></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">tiki-api</td>
      <td style="text-align:left">seller token key ( contact Tiki supporter )</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Path Parameters</b>
      </td>
      <td style="text-align:left"><b>Name</b>
      </td>
      <td style="text-align:left"><b>Type</b>
      </td>
      <td style="text-align:left"><b>Mandatory</b>
      </td>
      <td style="text-align:left"><b>Example</b>
      </td>
      <td style="text-align:left"><b>Description</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">order_code</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">Y</td>
      <td style="text-align:left">&quot;<code>20939384&quot;</code>
      </td>
      <td style="text-align:left">order_code of order you want to print</td>
    </tr>
    <tr>
      <td style="text-align:left">Query Parameters</td>
      <td style="text-align:left">Name</td>
      <td style="text-align:left">Type</td>
      <td style="text-align:left">Mandatory</td>
      <td style="text-align:left">Example</td>
      <td style="text-align:left">Description</td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">label</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">Y</td>
      <td style="text-align:left">&quot;invoice&quot;</td>
      <td style="text-align:left">
        <p>Label need download. Default is shipping label, option values:
          <br />
        </p>
        <ul>
          <li><b>shipping</b>: Get link shipping label</li>
          <li><b>invoice: </b>Get link invoice label.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">format</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">N</td>
      <td style="text-align:left">&quot;pdf&quot;</td>
      <td style="text-align:left">
        <p>Default is html, option values:</p>
        <ul>
          <li><b>html</b>
          </li>
          <li><b>pdf</b>(Just support label = invoice)</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>**4.7.2\) Response**

Http Status Code: 200

| Field | Type | Example | Description |
| :--- | :--- | :--- | :--- |
| shipping\_label\_url | String | "[http://uat.tikicdn.com/ts/print/1b/67/52/d54614ae10e18b2112c38845641a693d.html](http://uat.tikicdn.com/ts/print/1b/67/52/d54614ae10e18b2112c38845641a693d.html)" | Shipping label url |
| invoice\_label\_url | String | "[https://guat.tikicdn.com/ts/print/12/34/5d/669d74d1da850e337aeefdaf9aa9cef9.html](https://guat.tikicdn.com/ts/print/12/34/5d/669d74d1da850e337aeefdaf9aa9cef9.html)" | Invoice label url |

`Sample get shipping label: "GET /orders/833662058/print"`

```text
{
    "shipping_label_url": "http://uat.tikicdn.com/ts/print/1b/67/52/d54614ae10e18b2112c38845641a693d.html"
}
```

```text
Sample get invoice label: "GET /orders/833662058/print?label=invoice"

```

```text
{
    "invoice_label_url": "https://guat.tikicdn.com/ts/print/12/34/5d/669d74d1da850e337aeefdaf9aa9cef9.html"
}
```

**4.7.3\) Exception Case**

| HTTP Code | message | Description |
| :--- | :--- | :--- |
| 500 | Internal server error | having error in server, can't serving |
| 400 | Bad request | request not valid |

