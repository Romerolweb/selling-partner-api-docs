# Catalog Items API Use Case Guide
API Version: 2020-12-01
# Contents

* [What is the Catalog Items API?](#what-is-the-catalog-items-api)
* [Tutorial: Retrieve details for an item in the Amazon catalog](#tutorial-retrieve-details-for-an-item-in-the-amazon-catalog)
   * [Step 1: Get information about a catalog item](#step-1-get-information-about-a-catalog-item)
* [Tutorial: Search for items in the Amazon catalog](#tutorial-search-for-items-in-the-amazon-catalog)
   * [Step 1: Get a list of catalog items and associated information](#step-1-get-a-list-of-catalog-items-and-associated-information)
   * [Paging in the response to a search for items in the Amazon catalog](#paging-in-the-response-to-a-search-for-items-in-the-amazon-catalog)

# What is the Catalog Items API?

Using the Selling Partner API for Catalog Items (Catalog Items API), you can retrieve information about items in the Amazon catalog. See the [Catalog Items API Reference](https://github.com/amzn/selling-partner-api-docs/blob/main/references/catalog-items-api/catalogItems_2020-12-01.md) for details about API operations and associated data types and schemas.
	 
**Key Features**
* **Retrieve Detailed Item Information**: The Catalog Items API provides details about items in the Amazon catalog, such as summarized item details, product identifiers, sales rankings, variations, and thumbnail images. Vendors may retrieve additional vendor-specific details and brand owners of items may retrieve additional attributes and image content.
* **Search for Items**: The Catalog Items API allows you to search the Amazon catalog for existing items using keywords, including product identifiers.

**Terminology**
* **ASIN**: Amazon Standard Identification Number that identifies an item in the Amazon catalog.
* **Variation**: Every color or size for a catalog item represents a variation that is assigned a different ASIN. They are grouped together as variations of one parent ASIN.  
# Tutorial: Retrieve details for an item in the Amazon catalog

Use this tutorial to retrieve information about an item in the Amazon catalog for the given ASIN and marketplaces.
	 
**Prerequisites**

To complete this tutorial, you will need:
* Authorization from the Selling Partner for whom you are making calls. See the [Selling Partner API Developer Guide](https://github.com/amzn/selling-partner-api-docs/blob/main/guides/en-US/developer-guide/SellingPartnerApiDeveloperGuide.md) for more information.
* Approval for the Product Listing role in your developer profile.
* The Product Listing role selected in the App registration page for your application.  
## Step 1: Get information about a catalog item

Call the [getCatalogItem](https://github.com/amzn/selling-partner-api-docs/blob/main/references/catalog-items-api/catalogItems_2020-12-01.md#getcatalogitem) operation, passing the following parameters:
	 
**Request Parameters**

**Path Parameter**	 
<table width="100%">
  <thead>
    <tr class="header">
      <th>
        <b>Parameter</b>
      </th>
      <th>
        <b>Example</b>
      </th>
      <th>
        <b>Description</b>
      </th>
      <th>
        <b>Required</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr class="even">
      <td>
        <code>asin</code>
      </td>
      <td>
        <code>XXXXXXXXXX</code>
      </td>
      <td>Amazon Standard Identification Number for the item of interest.<p>Type: string</p></td>
      <td>Yes</td>
    </tr>
  </tbody>
</table>

**Query Parameters**

<table width="100%">
  <thead>
    <tr class="header">
      <th>
        <b>Parameter</b>
      </th>
      <th>
        <b>Example</b>
      </th>
      <th>
        <b>Description</b>
      </th>
      <th>
        <b>Required</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr class="odd">
      <td>
        <code>marketplaceIds</code>
      </td>
      <td>
        <code>ATVPDKIKX0DER</code>
      </td>
      <td>A comma-delimited list of Amazon marketplace identifiers.
        <br/>
        <br/>
        See the
        <a href="https://github.com/amzn/selling-partner-api-docs/blob/main/guides/en-US/developer-guide/SellingPartnerApiDeveloperGuide.md" target="_blank">Selling Partner API Developer Guide</a>
        for the list of Amazon marketplace identifiers.<p>Type: < string > array(csv)</p>
      </td>
      <td>Yes</td>
    </tr>
    <tr class="even">
      <td>
        <code>includedData</code>
      </td>
      <td>
        <code>summaries</code>
      </td>
      <td>A comma-delimited list of item details to request. If none are specified, will default to returning
        <code>summaries</code>
        data.
        <p>
          <ul>
            <li>
              <code>attributes</code>
              - Listable item attributes. Available to the brand owner of the item. Contains all listable item attributes present on the item for its particular product type.
            </li>
            <li>
              <code>identifiers</code>
              - External item identifiers such as EAN, UPC, ISBN, etc.
            </li>
            <li>
              <code>images</code>
              - Product images. Each product image will contain the name of the image variant, resolution, and a link to download the image. All requests for images will include the MAIN product image at small resolution. Requests from brand owners will additionally include other image variants at full resolution.
            </li>
            <li>
              <code>productTypes</code>
              - Item product type data. The category this product sells under in the Amazon marketplace.
            </li>
            <li>
              <code>salesRanks</code>
              - Item sales ranking data. Each sales ranking will contain the name of the category, the item's ranking, and a link to the sales ranking page on the retail website.
            </li>
            <li>
              <code>summaries</code>
              - Summary of item data. Basic attributes such as the item name, manufacturer, and brand.
            </li>
            <li>
              <code>variations</code>
              - Item variation data. Contains the list of the ASINs of the items related to this item and whether this item is a child or parent item.
            </li>
            <li>
              <code>vendorDetails</code>
              - Item vendor data. Available to vendors. Contains item replenishment, brand, and manufacturer information.
            </li>
          </ul>
        </p><p>Type: < enum (<a href="https://github.com/amzn/selling-partner-api-docs/blob/main/references/catalog-items-api/catalogItems_2020-12-01.md#includeddata-subgroup-2">IncludedData</a>) > array(csv)</p>
      </td>
      <td>No</td>
    </tr><tr class="odd">
      <td>
        <code>locale</code>
      </td>
      <td>
        <code>en_US</code>
      </td>
      <td>Locale for retrieving localized summaries. Defaults to the primary locale of the marketplace.<p>Type: string</p>
      </td>
      <td>No</td>
    </tr>
  </tbody>
</table>


**Example Request**
```plain
GET https://sellingpartnerapi-na.amazon.com/catalog/2020-12-01/items/XXXXXXXXXX
   ?marketplaceIds=ATVPDKIKX0DER
   &includedData=attributes,identifiers,images,productTypes,salesRanks,summaries,variations,vendorDetails
```

**Response**

A successful response includes the following:

<table width="100%">
  <thead>
    <tr class="header">
      <th>
        <b>Name</b>
      </th>
      <th>
        <b>Example</b>
      </th>
      <th>
        <b>Description</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr class="even">
      <td>
        <code>asin</code>
      </td>
      <td>
        <code>XXXXXXXXXX</code>
      </td>
      <td>The requested ASIN.<p>Type: <a href="https://github.com/amzn/selling-partner-api-docs/blob/main/references/catalog-items-api/catalogItems_2020-12-01.md#itemasin">ItemAsin</a></p></td>
    </tr>
    <tr class="odd">
      <td>
        <code>attributes</code>
      </td>
      <td>
        <em>See example response</em>
      </td>
      <td>A JSON object containing detailed catalog item data. Values from multiple marketplaces are rolled up into a list under each attribute name.<p>Type: <a href="https://github.com/amzn/selling-partner-api-docs/blob/main/references/catalog-items-api/catalogItems_2020-12-01.md#itemattributes">ItemAttributes</a></p></td>
    </tr>
    <tr class="even">
      <td>
        <code>identifiers</code>
      </td>
      <td>
        <em>See example response</em>
      </td>
      <td>External identifiers such as UPC, EAN, etc., if applicable.<p>Type: <a href="https://github.com/amzn/selling-partner-api-docs/blob/main/references/catalog-items-api/catalogItems_2020-12-01.md#itemidentifiers">ItemIdentifiers</a></p></td>
    </tr>
    <tr class="odd">
      <td>
        <code>images</code>
      </td>
      <td>
        <em>See example response</em>
      </td>
      <td>Image data for the item.<p>Type: <a href="https://github.com/amzn/selling-partner-api-docs/blob/main/references/catalog-items-api/catalogItems_2020-12-01.md#itemimages">ItemImages</a></p></td>
    </tr>
    <tr class="even">
      <td>
        <code>productTypes</code>
      </td>
      <td>
        <em>See example response</em>
      </td>
      <td>The product type category of the item within the Amazon catalog.<p>Type: <a href="https://github.com/amzn/selling-partner-api-docs/blob/main/references/catalog-items-api/catalogItems_2020-12-01.md#itemproducttypes">ItemProductTypes</a></p></td>
    </tr>
    <tr class="odd">
      <td>
        <code>ranks</code>
      </td>
      <td>
        <em>See example response</em>
      </td>
      <td>The sales ranking data for the item in every category it is tracked in.<p>Type: <a href="https://github.com/amzn/selling-partner-api-docs/blob/main/references/catalog-items-api/catalogItems_2020-12-01.md#itemsalesranks">ItemSalesRanks</a></p></td>
    </tr>
    <tr class="even">
      <td>
        <code>summaries</code>
      </td>
      <td>
        <em>See example response</em>
      </td>
      <td>Summary of item data.<p>Type: <a href="https://github.com/amzn/selling-partner-api-docs/blob/main/references/catalog-items-api/catalogItems_2020-12-01.md#itemsummaries">ItemSummaries</a></p></td>
    </tr>
    <tr class="odd">
      <td>
        <code>variations</code>
      </td>
      <td>
        <em>See example response</em>
      </td>
      <td>Other ASINs related to this one and whether this one is a parent ASIN or a child ASIN.<p>Type: <a href="https://github.com/amzn/selling-partner-api-docs/blob/main/references/catalog-items-api/catalogItems_2020-12-01.md#itemvariations">ItemVariations</a></p></td>
    </tr>
    <tr class="even">
      <td>
        <code>vendorDetails</code>
      </td>
      <td>
        <em>See example response</em>
      </td>
      <td>Detailed vendor information for this product.<p>Type: <a href="https://github.com/amzn/selling-partner-api-docs/blob/main/references/catalog-items-api/catalogItems_2020-12-01.md#itemvendordetails">ItemVendorDetails</a></p></td>
    </tr>
  </tbody>
</table>

​	 
**Example Response**


```plain
{
  "asin": "B07N4M94X4",
  "attributes": {
    "total_hdmi_ports": [
      {
        "value": 4,
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "resolution": [
      {
        "language_tag": "en_US",
        "value": "4K",
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "item_weight": [
      {
        "unit": "pounds",
        "value": 107.6,
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "product_subcategory": [
      {
        "value": "50400150",
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "item_dimensions": [
      {
        "width": {
          "unit": "inches",
          "value": 72.4
        },
        "length": {
          "unit": "inches",
          "value": 2.4
        },
        "height": {
          "unit": "inches",
          "value": 41.4
        },
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "brand": [
      {
        "language_tag": "en_US",
        "value": "Samsung Electronics",
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "generic_keyword": [
      {
        "language_tag": "en_US",
        "value": "smart tv; 4k tv; roku tv ;lg tv; oled tv; 65 inch smart tv; 4k tv 65 inch; lg smart tv; nvidia shield tv 2018; tv 4k; oled tv 65; sony 4k tv; 4k smart tv; 4k hdr tv; nvidia shield tv; gaming tv; lg 65 inch 4k tv; tv 65 inch smart tv 4k; 65 inch 4k tv; sony 65 inch 4k tv; vizio 4k tv; uhd tv; uhd tv 4k",
        "marketplace_id": "ATVPDKIKX0DER"
      },
      {
        "language_tag": "en_US",
        "value": "lg oled; 65 inch smart tv; samsung qled 75 inch tv; 85 inch 4k tv; lg smart tv; 4k tv 65 inch; samsung qled 82 inch tv; 8k tv; lg oled 65; lg smart tv; qled samsung 65 inch; 80 inch tv 4k; sony 4k tv; nvidia shield tv 2018",
        "marketplace_id": "ATVPDKIKX0DER"
      },
      {
        "language_tag": "en_US",
        "value": "samsung q9fn qled 2018; vizio; lg oled; lg 4k; sony 4k; sony oled; toshiba; antenna; dvd player; outdoor tv; kitchen tv; fire tv; firetv; hdtv; hd tv; android; shield tv; gaming; deals; tv ears; roku; dvr; speakers; digital tv antenna; apple tv; android tv; frame; mount",
        "marketplace_id": "ATVPDKIKX0DER"
      },
      {
        "language_tag": "en_US",
        "value": "4k hdr tv; 70" tv; nvidia shield tv; 90 inch tv; gaming tv; 75" tv; lg 65 inch 4k tv; tv 65 inch smart tv 4k; 65 inch 4k tv; sony 65 inch 4k tv; vizio 4k tv; uhd tv; uhd tv 4k;",
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "control_method": [
      {
        "value": "voice",
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "item_package_dimensions": [
      {
        "length": {
          "unit": "centimeters",
          "value": 26.67
        },
        "width": {
          "unit": "centimeters",
          "value": 121.92
        },
        "height": {
          "unit": "centimeters",
          "value": 203.2
        },
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "image_aspect_ratio": [
      {
        "language_tag": "en_US",
        "value": "16:9",
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "part_number": [
      {
        "value": "QN82Q60RAFXZA",
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "includes_remote": [
      {
        "value": true,
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "item_type_name": [
      {
        "language_tag": "en_US",
        "value": "TV",
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "battery": [
      {
        "cell_composition": [
          {
            "value": "alkaline"
          }
        ],
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "manufacturer": [
      {
        "language_tag": "en_US",
        "value": "Samsung",
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "number_of_boxes": [
      {
        "value": 1,
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "total_usb_ports": [
      {
        "value": 2,
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "model_number": [
      {
        "value": "QN82Q60RAFXZA",
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "supplier_declared_dg_hz_regulation": [
      {
        "value": "not_applicable",
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "num_batteries": [
      {
        "quantity": 2,
        "type": "aaa",
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "california_proposition_65": [
      {
        "compliance_type": "on_product_combined_cancer_reproductive",
        "marketplace_id": "ATVPDKIKX0DER"
      },
      {
        "compliance_type": "chemical",
        "chemical_names": [
          "di_2_ethylhexyl_phthalate_dehp"
        ],
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "display": [
      {
        "resolution_maximum": [
          {
            "unit": "pixels",
            "language_tag": "en_US",
            "value": "3840 x 2160"
          }
        ],
        "size": [
          {
            "unit": "inches",
            "value": 82
          }
        ],
        "type": [
          {
            "language_tag": "en_US",
            "value": "QLED"
          }
        ],
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "item_name": [
      {
        "language_tag": "en_US",
        "value": "Samsung QN82Q60RAFXZA Flat 82-Inch QLED 4K Q60 Series (2019) Ultra HD Smart TV with HDR and Alexa Compatibility",
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "list_price": [
      {
        "currency": "USD",
        "value": 3799.99,
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "batteries_required": [
      {
        "value": false,
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "includes_rechargable_battery": [
      {
        "value": false,
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "product_site_launch_date": [
      {
        "value": "2019-03-11T08:00:01.000Z",
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "product_category": [
      {
        "value": "50400100",
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "batteries_included": [
      {
        "value": false,
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "connectivity_technology": [
      {
        "language_tag": "en_US",
        "value": "Bluetooth",
        "marketplace_id": "ATVPDKIKX0DER"
      },
      {
        "language_tag": "en_US",
        "value": "USB",
        "marketplace_id": "ATVPDKIKX0DER"
      },
      {
        "language_tag": "en_US",
        "value": "Wireless",
        "marketplace_id": "ATVPDKIKX0DER"
      },
      {
        "language_tag": "en_US",
        "value": "HDMI",
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "included_components": [
      {
        "language_tag": "en_US",
        "value": "QLED Standard Smart Remote",
        "marketplace_id": "ATVPDKIKX0DER"
      },
      {
        "language_tag": "en_US",
        "value": "Power Cable",
        "marketplace_id": "ATVPDKIKX0DER"
      },
      {
        "language_tag": "en_US",
        "value": "Stand",
        "marketplace_id": "ATVPDKIKX0DER"
      },
      {
        "language_tag": "en_US",
        "value": "Samsung Smart Control",
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "specification_met": [
      {
        "language_tag": "en_US",
        "value": "",
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "cpsia_cautionary_statement": [
      {
        "value": "no_warning_applicable",
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "item_type_keyword": [
      {
        "value": "qled-televisions",
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "number_of_items": [
      {
        "value": 1,
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "warranty_description": [
      {
        "language_tag": "en_US",
        "value": "1 year manufacturer",
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "max_resolution": [
      {
        "unit": "pixels",
        "value": 8.3,
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "item_package_weight": [
      {
        "unit": "kilograms",
        "value": 62.142,
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "supported_internet_services": [
      {
        "language_tag": "en_US",
        "value": "Amazon Instant Video",
        "marketplace_id": "ATVPDKIKX0DER"
      },
      {
        "language_tag": "en_US",
        "value": "YouTube",
        "marketplace_id": "ATVPDKIKX0DER"
      },
      {
        "language_tag": "en_US",
        "value": "Netflix",
        "marketplace_id": "ATVPDKIKX0DER"
      },
      {
        "language_tag": "en_US",
        "value": "Hulu",
        "marketplace_id": "ATVPDKIKX0DER"
      },
      {
        "language_tag": "en_US",
        "value": "Browser",
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "tuner_technology": [
      {
        "language_tag": "en_US",
        "value": "Analog Tuner",
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "wireless_communication_technology": [
      {
        "language_tag": "en_US",
        "value": "Wi-Fi::Wi-Fi Direct::Bluetooth",
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "model_year": [
      {
        "value": 2019,
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "power_source_type": [
      {
        "language_tag": "en_US",
        "value": "Corded Electric",
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "street_date": [
      {
        "value": "2019-03-21T00:00:01Z",
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ],
    "refresh_rate": [
      {
        "unit": "hertz",
        "language_tag": "en_US",
        "value": "120",
        "marketplace_id": "ATVPDKIKX0DER"
      }
    ]
  },
  "identifiers": [
    {
      "marketplaceId": "ATVPDKIKX0DER",
      "identifiers": [
        {
          "identifier": "0887276302195",
          "identifierType": "EAN"
        },
        {
          "identifier": "00887276302195",
          "identifierType": "GTIN"
        },
        {
          "identifier": "887276302195",
          "identifierType": "UPC"
        }
      ]
    }
  ],
  "images": [
    {
      "marketplaceId": "ATVPDKIKX0DER",
      "images": [
        {
          "variant": "MAIN",
          "link": "https://m.media-amazon.com/images/I/51DZzp3w3vL.jpg",
          "height": 333,
          "width": 500
        }
      ]
    }
  ],
  "productTypes": [
    {
      "marketplaceId": "ATVPDKIKX0DER",
      "productType": "TELEVISION"
    }
  ],
  "ranks": [
    {
      "marketplaceId": "ATVPDKIKX0DER",
      "ranks": [
        {
          "title": "Electronics",
          "link": "http://www.amazon.com/gp/bestsellers/electronics",
          "value": 61667
        },
        {
          "title": "QLED TVs",
          "link": "http://www.amazon.com/gp/bestsellers/electronics/21489946011",
          "value": 84
        }
      ]
    }
  ],
  "summaries": [
    {
      "marketplaceId": "ATVPDKIKX0DER",
      "brandName": "Samsung Electronics",
      "colorName": "Black",
      "itemName": "Samsung QN82Q60RAFXZA Flat 82-Inch QLED 4K Q60 Series (2019) Ultra HD Smart TV with HDR and Alexa Compatibility",
      "manufacturer": "Samsung",
      "modelNumber": "QN82Q60RAFXZA",
      "sizeName": "82-Inch",
      "styleName": "TV only"
    }
  ],
  "variations": [
    {
      "marketplaceId": "ATVPDKIKX0DER",
      "asins": [
        "B08J7TQ9FL"
      ],
      "variationType": "CHILD"
    }
  ],
  "vendorDetails": [
    {
      "marketplaceId": "ATVPDKIKX0DER",
      "brandCode": "SAMF9",
      "categoryCode": "50400100",
      "manufacturerCode": "SAMF9",
      "manufacturerCodeParent": "SAMF9",
      "productGroup": "Home Entertainment",
      "replenishmentCategory": "OBSOLETE",
      "subcategoryCode": "50400150"
    }
  ]
}
```

# Tutorial: Search for items in the Amazon catalog

Use this tutorial to search for items in the Amazon catalog.
	 
**Prerequisites**

To complete this tutorial, you will need:
* Authorization from the Selling Partner for whom you are making calls. See the [Selling Partner API Developer Guide](https://github.com/amzn/selling-partner-api-docs/blob/main/guides/en-US/developer-guide/SellingPartnerApiDeveloperGuide.md) for more information.
* Approval for the Product Listing role in your developer profile.
* The Product Listing role selected in the App registration page for your application.  
## Step 1: Get a list of catalog items and associated information

To search for and return a list of catalog items with the optional associated information you specify, call the searchCatalogItems operation.

Call the [searchCatalogItems](https://github.com/amzn/selling-partner-api-docs/blob/main/references/catalog-items-api/catalogItems_2020-12-01.md#searchcatalogitems) operation, passing the following parameters:
	 
**Request Parameters**

**Query Parameters**

<table width="100%">
  <thead>
    <tr class="header">
      <th>
        <b>Parameter</b>
      </th>
      <th>
        <b>Example</b>
      </th>
      <th>
        <b>Description</b>
      </th>
      <th>
        <b>Required</b>
      </th>
    </tr>
  </thead>
  <tbody>
  <tr class="odd">
      <td>
        <code>keywords</code>
      </td>
      <td>
        <code>shoes</code>
      </td>
      <td>A comma-delimited list of words or item identifiers to search the Amazon catalog for.
        <p>Type: < string > array(csv)</p>
      </td>
      <td>Yes</td>
    </tr>
    <tr class="even">
      <td>
        <code>marketplaceIds</code>
      </td>
      <td>
        <code>ATVPDKIKX0DER</code>
      </td>
      <td>A comma-delimited list of Amazon marketplace identifiers.
        <br/>
        <br/>
        See the
        <a href="https://github.com/amzn/selling-partner-api-docs/blob/main/guides/en-US/developer-guide/SellingPartnerApiDeveloperGuide.md" target="_blank">Selling Partner API Developer Guide</a>
        for the list of Amazon marketplace identifiers.<p>Type: < string > array(csv)</p>
      </td>
      <td>Yes</td>
    </tr>
    <tr class="odd">
      <td>
        <code>includedData</code>
      </td>
      <td>
        <code>summaries</code>
      </td>
      <td>A comma-delimited list of item details to request. If none are specified, will default to returning
        <code>summaries</code>
        data.
        <p>
          <ul>
            <li>
              <code>identifiers</code>
              - External item identifiers such as EAN, UPC, ISBN, etc.
            </li>
            <li>
              <code>images</code>
              - Product images. Each product image will contain the name of the image variant, resolution, and a link to download the image. All requests for images will include the MAIN product image at small resolution. Requests from brand owners will additionally include other image variants at full resolution.
            </li>
            <li>
              <code>productTypes</code>
              - Item product type data. The category this product sells under in the Amazon marketplace.
            </li>
            <li>
              <code>salesRanks</code>
              - Item sales ranking data. Each sales ranking will contain the name of the category, the item's ranking, and a link to the sales ranking page on the retail website.
            </li>
            <li>
              <code>summaries</code>
              - Summary of item data. Basic attributes such as the item name, manufacturer, and brand.
            </li>
            <li>
              <code>variations</code>
              - Item variation data. Contains the list of the ASINs of the items related to this item and whether this item is a child or parent item.
            </li>
            <li>
              <code>vendorDetails</code>
              - Item vendor data. Available to vendors. Contains item replenishment, brand, and manufacturer information.
            </li>
          </ul>
        </p><p>Type: < enum (<a href="https://github.com/amzn/selling-partner-api-docs/blob/main/references/catalog-items-api/catalogItems_2020-12-01.md#includeddata-subgroup-1">IncludedData</a>) > array(csv)</p>
      </td>
      <td>No</td>
    </tr>
    <tr class="even">
      <td>
        <code>brandNames</code>
      </td>
      <td>
        <code>Beautiful Boats</code>
      </td>
      <td>A comma-delimited list of brand names to limit the search to.<p>Type: < string > array(csv)</p></td>
      <td>No</td>
    </tr>
    </tr>
    <tr class="odd">
      <td>
        <code>classificationIds</code>
      </td>
      <td>
        <code>12345678</code>
      </td>
      <td>A comma-delimited list of classification identifiers to limit the search to.<p>Type: < string > array(csv)</p></td>
      <td>No</td>
    </tr>
    </tr>
    <tr class="even">
      <td>
        <code>pageSize</code>
      </td>
      <td>
        <code>9</code>
      </td>
      <td>Number of results to be returned per page. <p>Default: 10</p><p>Type: integer</p></td>
      <td>No</td>
    </tr>
    </tr>
    <tr class="odd">
      <td>
        <code>pageToken</code>
      </td>
      <td><em>See example response.</em></td>
      <td>A token to fetch a certain page when there are multiple pages worth of results.<p>Type: string</p></td>
      <td>No</td>
    </tr>
    </tr>
    <tr class="even">
      <td>
        <code>keywordsLocale</code>
      </td>
      <td>
        <code>en-US</code>
      </td>
      <td>The language the keywords are provided in. Keywords will be translated to the response locale for searching if the two differ.<p>Type: string</p></td>
      <td>No</td>
    </tr>
    </tr>
    <tr class="odd">
      <td>
        <code>locale</code>
      </td>
      <td>
        <code>es-US</code>
      </td>
      <td>Return results in this language, if available. If this parameter is not specified, the results will be returned in the marketplace's default language.<p>Type: string</p></td>
      <td>No</td>
    </tr>
  </tbody>
</table>

**Example Request**

```plain
GET https://sellingpartnerapi-na.amazon.com/catalog/2020-12-01/items
   ?keywords=red,polo,shirt
   &marketplaceIds=ATVPDKIKX0DER
   &includedData=summaries
   &pageSize=5
```

**Response**

A successful response includes the following:

<table width="100%">
  <thead>
    <tr class="header">
      <th>
        <b>Name</b>
      </th>
      <th>
        <b>Example</b>
      </th>
      <th>
        <b>Description</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr class="even">
      <td>
        <code>numberOfResults</code>
      </td>
      <td>
        <code>3097</code>
      </td>
      <td>The total number of products matched by the search query (only results up to the page count limit will be returned per request regardless of the number found).<p>Type: integer</p></td>
    </tr>
    <tr class="odd">
      <td>
        <code>pagination</code>
      </td>
      <td>
        <em>See example response</em>
      </td>
      <td>A JSON object containing one or more page tokens that can be used to fetch the next or previous page of results.<p>Type: <a href="https://github.com/amzn/selling-partner-api-docs/blob/main/references/catalog-items-api/catalogItems_2020-12-01.md#pagination">Pagination</a></p></td>
    </tr>
    <tr class="even">
      <td>
        <code>refinements</code>
      </td>
      <td>
        <em>See example response</em>
      </td>
      <td>A JSON object containing keys that can be used to refine the search results down to certain brands or categories.<p>Type: <a href="https://github.com/amzn/selling-partner-api-docs/blob/main/references/catalog-items-api/catalogItems_2020-12-01.md#refinements">Refinements</a></p></td>
    </tr>
    <tr class="odd">
      <td>
        <code>items</code>
      </td>
      <td>
        <em>See example response</em>
      </td>
      <td>A list of items from the Amazon catalog.<p>Type: < <a href="https://github.com/amzn/selling-partner-api-docs/blob/main/references/catalog-items-api/catalogItems_2020-12-01.md#item">Item</a> > array</p></td>
    </tr>
  </tbody>
</table>


​	 
**Example Response**


```plain
{
    "numberOfResults": 12247,
    "pagination": {
        "nextToken": "9HkIVcuuPmX_bm51o3-igBfN45pxW4Ru7ElIM6GCECYCuXJKzT26f-AlJJZYjIPp8wkOEmQdma1wt_JvE7qiRmNsKy7hH"
    },
    "refinements": {
        "brands": [
            {
                "numberOfResults": 91,
                "brandName": "Polo Ralph Lauren"
            },
            {
                "numberOfResults": 79,
                "brandName": "Eddie Bauer"
            },
            {
                "numberOfResults": 46,
                "brandName": "Cutter & Buck"
            },
            {
                "numberOfResults": 39,
                "brandName": "FILA"
            },
            {
                "numberOfResults": 37,
                "brandName": "Orvis"
            }
        ],
        "classifications": [
            {
                "numberOfResults": 1243,
                "displayName": "Clothing, Shoes & Jewelry",
                "classificationId": "7141124011"
            },
            {
                "numberOfResults": 126,
                "displayName": "Sports & Outdoors",
                "classificationId": "3375301"
            }
        ]
    },
    "items": [
        {
            "asin": "B002N36Q3M",
            "summaries": [
                {
                    "marketplaceId": "ATVPDKIKX0DER",
                    "brandName": "Fred Perry",
                    "colorName": "Wht/Brt Red/Nvy",
                    "itemName": "Fred Perry Men's Twin Tipped Polo Shirt-M1200, WHT/BRT RED/NVY, X-Large",
                    "manufacturer": "Fred Perry Men's Apparel",
                    "modelNumber": "M1200",
                    "sizeName": "X-Large",
                    "styleName": "Twin Tipped Polo Shirt-m1200"
                }
            ]
        },
        {
            "asin": "B002N3ABSI",
            "summaries": [
                {
                    "marketplaceId": "ATVPDKIKX0DER",
                    "brandName": "Fred Perry",
                    "colorName": "White/Bright Red/Navy",
                    "itemName": "Fred Perry Men's Twin Tipped Polo, White/Bright Red/Navy, SM",
                    "manufacturer": "Fred Perry Apparel Mens",
                    "modelNumber": "M1200-748",
                    "sizeName": "SM",
                    "styleName": "Twin Tipped Fred Perry Polo"
                }
            ]
        },
        {
            "asin": "B01N5B3598",
            "summaries": [
                {
                    "marketplaceId": "ATVPDKIKX0DER",
                    "brandName": "NHL",
                    "colorName": "Red",
                    "itemName": "NHL New Jersey Devils Men's Polo, Small, Red",
                    "manufacturer": "Knight's Apparel",
                    "modelNumber": "H0MEE3ZAMZ",
                    "sizeName": "Small"
                }
            ]
        },
        {
            "asin": "B00HIVDUXI",
            "summaries": [
                {
                    "marketplaceId": "ATVPDKIKX0DER",
                    "brandName": "adidas",
                    "colorName": "Bold Red/White",
                    "itemName": "Adidas Golf Men's Puremotion Textured Stripe Polo, Bold Red/White, Large",
                    "manufacturer": "TaylorMade - Adidas Golf Apparel",
                    "modelNumber": "TM3010S4",
                    "sizeName": "Large"
                }
            ]
        },
        {
            "asin": "B005ZZ12YS",
            "summaries": [
                {
                    "marketplaceId": "ATVPDKIKX0DER",
                    "brandName": "RALPH LAUREN",
                    "colorName": "Black / Red Pony",
                    "itemName": "Polo Ralph Lauren Men's Long-sleeved T-shirt / Sleepwear / Thermal in Black, Red Pony (Large / L)",
                    "sizeName": "Large"
                }
            ]
        }
    ]
}
```

## Paging in the response to a search for items in the Amazon catalog
When a call to the [searchCatalogItems](https://github.com/amzn/selling-partner-api-docs/blob/main/references/catalog-items-api/catalogItems_2020-12-01.md#searchcatalogitems) operation produces a response that exceeds the `pageSize`, pagination occurs. This means the response is divided into individual pages, where each page is returned in successive calls. To retrieve the next page or the previous page, you must pass the `nextToken` value or the `previousToken` value as the `pageToken` parameter in the next request. 

You get the first page of results when you call the searchCatalogItems operation and provide no page token. You then iterate through the rest of the pages using the `nextToken` page token provided in successive responses. 

Page tokens are special values that are decoded to determine which page is requested and how many pages are before or after.

If the next or previous page is not available, the corresponding page token attribute (`nextToken` or `previousToken` respectively) will not be present in the `pagination` object. 

Examples:

When the response does not exceed the `pageSize`, there is no pagination, so there is no `nextToken` or `previousToken`:

```plain
"pagination": {
},
```

When the response exceeds the `pageSize` and pagination occurs:

For the first page, there is no previous page, so there is no `previousToken`:

```plain
"pagination": {
  "nextToken": "XXXXXX"
},
```

For the last page, there is no next page, so there is no `nextToken`:

```plain
"pagination": {
  "previousToken": "XXXXXX"
},
```

For all other pages:

```plain
"pagination": {
  "nextToken": "XXXXXX",
  "previousToken": "XXXXXX"
},
```

Note: Even though there can be more than 1000 ASINs that match the search criteria, the maximum number of results that can be returned and paged through is limited to 1000. For example, if the caller sets the `pageSize` to 10, the maximum number of possible pages is 100.