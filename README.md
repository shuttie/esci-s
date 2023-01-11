# ESCI-S: extended metadata for Amazon ESCI dataset

[![License: Apache 2](https://img.shields.io/badge/License-Apache2-green.svg)](https://opensource.org/licenses/Apache-2.0)


This is a complimentary dataset of product metadata for the original [Amazon ESCI](https://github.com/amazon-science/esci-data) dataset.
> ESCI is a large dataset of difficult search queries, released with the aim of fostering research in the area of semantic matching of queries and products. For each query, the dataset provides a list of up to 40 potentially relevant results, together with ESCI relevance judgements (Exact, Substitute, Complement, Irrelevant) indicating the relevance of the product to the query. Each query-product pair is accompanied by additional product information.

## Data

* Full dataset, 1.66M products, 3.4G [Zstd](https://github.com/facebook/zstd) compressed: [S3](https://esci-s.s3.amazonaws.com/esci.json.zst) [GDrive](https://drive.google.com/file/d/1HZLeUg61GWU3A6xyUEIw9AJtR-AwK5aw/view?usp=share_link)
* 0.25% sample, 4400 products, 10M compressed: [Github LFS](https://github.com/shuttie/esci-s/raw/master/sample.json.gz)

## Motivation

As an e-commerce dataset, the ESCI is focused on text information about products and queries, **leaving behavioral, numerical and categorical ranking features on the side**. The original product information includes the following fields:
* product_id
* product_title
* product_description
* product_bullet_point
* product_brand
* product_color
* product_locale

It references ~1.8M unique products, and our goal was to enrich product metadata with non-textual information, which is typical for e-commerce applications. 

## Methodology

As product_id is technically an [ASIN](https://en.wikipedia.org/wiki/Amazon_Standard_Identification_Number), a global amazon-wide product identifier, so we scraped and parsed all 1.8M referenced products, extending the original ESCI dataset with the following extra fields:
* `type` - is it a book or a product?
* `stars` - average review score
* `ratings` - number of reviews
* `category` - list of categories (from top to bottom)
* `attributes` - a map of product attributes, displayed under the reviews block.
* `bullets` - a list of product bullet points, should be the same as original `product_bullet_point` field
* `description` - product main description
* `info` - an extended map of product attributes, displayed under the description.
* `reviews` - a structured set of review data (`stars`, `title`, `date`, `text`)
* `price` - original price
* `formats` - a map of available product formats (for example, kindle and paperback forms with their prices)
* `template` - page layout template, usually correlated with the top category
* `image` - URL of the main image
* `locale` - main language of the page

Books do have a couple of specific extra fields:
* `subtitle` - a secondary title
* `author` - book author name
* `review` - extra book review from Amazon

## Example

A product example:

```json
{
  "type": "product",
  "locale": "us",
  "asin": "B06XXXLJ6V",
  "title": "FYY Leather Case with Mirror for Samsung Galaxy S8 Plus, Leather Wallet Flip Folio Case with Mirror and Wrist Strap for Samsung Galaxy S8 Plus Black",
  "stars": "4.3 out of 5 stars",
  "ratings": "1,116 ratings",
  "category": [
    "Cell Phones & Accessories",
    "Cases, Holsters & Sleeves",
    "Flip Cases"
  ],
  "attrs": {
    "Material": "Faux Leather",
    "Compatible Phone Models": "Samsung Galaxy S8 Plus",
    "Form Factor": "Flip",
    "Brand": "FYY",
    "Color": "Black"
  },
  "bullets": [
    "RFID Technique: Radio Frequency Identification technology, through radio signals to identify specific targets and to read and copy electronic data. Most Credit Cards, Debit Cards, ID Cards are set-in the RFID chip, the RFID reader can easily read the cards information within 10 feet(about 3m) without touching them. This case is designed to protect your cards information from stealing with blocking material of RFID shielding technology.",
    "Unique design. Cosmetic Mirror inside made for your makeup and beauty.",
    "Card slots provide you to put debit card, credit card or ID card while on the go.",
    "High quality. Made with Premium PU Leather. Kickstand function is convenient for movie-watching or video-chatting.",
    "Perfect gift for Mother's Day, Father's Day, Valentine's Day, Thanksgiving Day and Christmas."
  ],
  "description": "Product Description Premium PU Leather Top quality. Made with Premium PU Leather. Receiver design. Accurate cut-out for receiver. Convenient to Answer the phone without open the case. Hand strap makes it easy to carry around. RFID Technique RFID Technique: Radio Frequency Identification technology, through radio signals to identify specific targets and to read and copy electronic data. Most Credit Cards, Debit Cards, ID Cards are set-in the RFID chip, the RFID reader can easily read the cards information within 10 feet(about 3m) without touching them. This case is designed to protect your cards information from stealing with blocking material of RFID shielding technology. 100% Handmade 100% Handmade. Perfect craftmanship and reinforced stitching makes it even more durable. Sleek, practical and elegant with a variety of dashing colors. Multiple Functions Card slots are designed for you to put your photo, debit card, credit card or ID card while on the go. Unique design. Cosmetic Mirror inside made for your makeup and beauty. Perfect Viewing Angle. Kickstand function is convenient for movie-watching or video-chatting. Space amplification, convenient to unlock. Kickstand function is convenient for movie-watching or video-chatting. ",
  "info": {
    "Included Components": "Kickstand",
    "Best Sellers Rank": "#141,895 in Cell Phones & Accessories ( See Top 100 in Cell Phones & Accessories ) #10,494 in Flip Cell Phone Cases",
    "Colour": "Black",
    "Product Dimensions": "15.99 x 8.99 x 1 inches",
    "Manufacturer": "GUANGZHOU WENYI COMMUNICATION EQIPMENT CO.,LTD",
    "Item Weight": "0.023 ounces",
    "Date First Available": "March 30, 2017",
    "ASIN": "B06XXXLJ6V",
    "Item model number": "4326500860",
    "Customer Reviews": "4.3 out of 5 stars 1,116 ratings 4.3 out of 5 stars",
    "Is Discontinued By Manufacturer": "No",
    "Form Factor": "Flip",
    "Other display features": "Wireless"
  },
  "reviews": [
    {
      "stars": "3.0 out of 5 stars",
      "title": "very cute but....",
      "date": "Reviewed in the United States 🇺🇸n September 22, 2022",
      "text": "very cute but didn't last long at all"
    },
    {
      "stars": "4.0 out of 5 stars",
      "title": "A stylish case",
      "date": "Reviewed in the United States 🇺🇸n July 13, 2017",
      "text": "Overall a nice looking case. Magnet is relatively strong to keep it closed. Another nice feature is that the 2 sides of the case are also magnetized, so they hold when you fold the top under/behind the phone (so it doesn't flap around). This was a pleasant surprise, and then you can secure it with the tab as well. I agree that the mirror is not as useful, since it's more of a fun house mirror (image is distorted). But if you're using it up close for lipstick or to get something out of your teeth or eye or to spy on the person behind you, it'll do fine. :-) The wallet is stylish and the material feels good, not like cheap or inflexible plastic. But the accent color is not silver like it appears, it's more of a glittery or metallic gold. It's nice though. I also agree with others that if you were to drop your phone, the case may open up, as the magnet is strong, but it's not fully secured closed. I don't plan on testing that though! You should still get a screen protector. The only downside to the wallet case is that the wrist strap is at the top of the case. I have a Galaxy S8+, which is really long. When I wear the strap around my wrist, it's almost impossible to hold the phone in the same hand. I commute standing on a train, so one hand use of the phone is important, as well as the safety net to wearing the wristlet on my wrist. I've tried with both hands. You just can't get to the keyboard to even Swype. Placing the strap at the bottom of the case would be more effective and useful, especially on the larger phones. I'd give the case 5 stars if the strap were placed at the bottom and more useable."
    },
    {
      "stars": "5.0 out of 5 stars",
      "title": "Great protection for my 8 s plus.",
      "date": "Reviewed in the United States 🇺🇸n July 19, 2022",
      "text": "Fit and color are great. Holds a few credit cards."
    },
    {
      "stars": "5.0 out of 5 stars",
      "title": "It fits my phone like a glove.",
      "date": "Reviewed in the United States 🇺🇸n June 20, 2022",
      "text": "It fits very well good quality and just a good buy for the money Fast shipping good seller"
    },
    {
      "stars": "5.0 out of 5 stars",
      "title": "phone case",
      "date": "Reviewed in the United States 🇺🇸n June 7, 2022",
      "text": "I love the case I still have it(great value for your money)"
    },
    {
      "stars": "4.0 out of 5 stars",
      "title": "I have this for a Few Months",
      "date": "Reviewed in the United States 🇺🇸n April 22, 2019",
      "text": "I got this around Spring break, so I have had it for a good 2 months almost. And I personally am really hard on my phone cases. I swing this around and am constantly playing with the magnet closing. That being said, the magnet on this case is VERY good. But, I have managed to tear up the side. First thing I noticed when it came, was the cute quote where the phone goes, a big appreciation for that. It isn't GREAT quality, but definitely really good when you compare the case to the price of what it was. The pockets in the case are a little tight at first, I stretched mine by making sure I could move my finger in them. The strap hole is very durable. The only complaint I might have, is the case makes it difficult to take rear face pictures. I usually end up popping my phone out for that, but overall I actually really love this case :)"
    },
    {
      "stars": "4.0 out of 5 stars",
      "title": "Great case",
      "date": "Reviewed in the United States 🇺🇸n June 18, 2021",
      "text": "I have bought many of cases though the years and every time I got them I have been disappointed, some have been hard to even put on the phone or too bulky or just not that attractive but this case changed everything about previous phone cases it is easy to put on the phone, it's beautiful as well as extremely durable and I will not buy another kind of phone case again."
    },
    {
      "stars": "5.0 out of 5 stars",
      "title": "Nice, holds cards, sleek look",
      "date": "Reviewed in the United States 🇺🇸n December 27, 2019",
      "text": "I have a galaxy 8+. This keeps it covered nicely. Has a tiny magnet to secure the side. The pockets get stretched so cards can fall out if the case gets upside down. The binding gets soft over time so it doesn't act as a good stand on its own. I lean it on something. I'd like to see the same magnet flap over the card area to keep them from falling out. Over all is a good case for the money, very attractive sleek look."
    },
    {
      "stars": "4.0 out of 5 stars",
      "title": "Ok mobile phone cover",
      "date": "Reviewed in Australia 🇦🇺n May 21, 2022",
      "text": "Nicely designed, ok storage but not best quality."
    }
  ],
  "price": "",
  "formats": {},
  "template": "wireless",
  "image": "https://m.media-amazon.com/images/I/81bdoltQWVL.__AC_SY300_SX300_QL70_FMwebp_.jpg"
}
```

## Statistics

* Total scraped products: **1661908 ASINs**
* Coverage: **91.5%** (original ESCI dataset includes 1814925 ASINs)
* Books: **7%, 117828 ASINs** 
* Page template statistics, top-10 categories:
  * apparel - 15.06%
  * kitchen - 8.33%
  * home - 5.86%
  * home_improvement - 5.28%
  * sports - 5.24%
  * book - 5.12%
  * beauty - 4.68%
  * shoes = 4.41%
  * health_and_beauty - 4.39%
  * toy - 4.39%


## License

This project is licensed under the Apache-2.0 License.