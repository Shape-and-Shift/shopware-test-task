# Shopware 6 Branches Entity Test Task

first of all  you very much for your job application. 

We really appreciate your interest in working at Shape & Shift! We would like to know more about your development skills and have a closer look at them. I would like to send you a backend task to find out your skills. Hoping that is okay for you.

Also, what is your salary expectations and when would be the earliest possible date to start?

Please use for further communication hello@shapeandshift.dev

### What are we acutally doing?
As a small team we're working within the background for primarily German [Shopware 6](https://www.shopware.com/en/) agencies as an experienced external team.
[Shopware 6](https://www.shopware.com/en/) is a headless E-Commerce System based on Symfony 4 & Vue.js for the administration
and Bootstrap, ES6 and Twig for the Storefront.

You can head over to the [Shopware 6 Documentation](https://docs.shopware.com/en/shopware-platform-dev-en) to take a closer look into the System itself.

### The Task
The test task is to create an entity to administrate branches within the administration.
You have to create the Shopware Plugin, Entity and last but not least the actual administration vue component.

Please create a private Gitlab-repository at https://gitlab.com/, where you can track progress and commit your work at the end of the day. Please make sure it is not visible to public and you give only access to the group “shape-and-shift” https://gitlab.com/shape-and-shift .

You can find the documentation under https://docs.shopware.com/en/shopware-platform-dev-en

#### Installation
- [Installation of the development template](https://docs.shopware.com/en/shopware-platform-dev-en/system-guide/installation?category=shopware-platform-dev-en/system-guide)
- [Difference between the development and production template](https://www.p16r.nl/2020-08-28-shopware-6-development-versus-production-template/)

> The development template offers you some development helpers such as a watcher like `psh.phar administration:watch` for the administration or a `psh.phar storefront:hot-proxy` server for the storefront. You can get all available commands with `./psh.phar`.

#### Helpfulp links:
- [Developer Video course](https://academy.shopware.com/collections?category=developer-sw6)
- [Shopware DAL](https://docs.shopware.com/en/shopware-platform-dev-en/developer-guide/database?category=shopware-platform-dev-en/developer-guide)
- [Bundle example: An InDepth guide](https://docs.shopware.com/en/shopware-platform-dev-en/how-to/indepth-guide-bundle)
- [Vue Component library](https://component-library.shopware.com/)
- [Create an admin module](https://docs.shopware.com/en/shopware-platform-dev-en/how-to/custom-module?category=shopware-platform-dev-en/how-to)

The new navigation point "Branches -> Overview" should be between customers and content within the administartion. 
It's described within the documentation [how you add this navigation](https://docs.shopware.com/en/shopware-platform-dev-en/how-to/custom-module#navigation) within your admin module. If you wonder how you can specify the order of the menu, take a look at some core module how it's done.

Now let's get started:

**Plugin name:** SasBranches
>You can run `bin/console plugin:create` to create your plugin via the CLI

**Entity Name**: `sas_branch`

The following data should be maintained:

| Section | Label | Type | Required | Comment |
|-|-|-|-|-|
| Branch | Name | varchar(255) | yes |  |
| Branch | Street | varchar(255) | yes |  |
| Branch | Postcode | varchar(255) | yes |  |
| Branch | City | varchar(255) | yes |  |
| Branch | Country | binary(16) | yes | relation to country table |
| Contact | Phone | varchar(255) | no |  |
| Contact | Email | varchar(255) | yes |  |

> The section is basically a new [card component](https://component-library.shopware.com/components/sw-card) within the detail [admin module](https://docs.shopware.com/en/shopware-platform-dev-en/how-to/custom-module?category=shopware-platform-dev-en/how-to)

![](https://res.cloudinary.com/dtgdh7noz/image/upload/v1607674279/Bildschirmfoto_2020-12-11_um_10.10.06_tn5zfx.png)
*Dummy image, the sections are not correct here*

The admin module should contain the overview page, detail & create page.

All data (name, street, ...) should be displayed on the overview page.
For the overview page please use the [sw-entity-listing](https://component-library.shopware.com/components/sw-entity-listing) component.
I can't say it enough - Always take a look at the existing core files, for example within the [sw-customer-list](https://github.com/shopware/platform/blob/master/src/Administration/Resources/app/administration/src/module/sw-customer/page/sw-customer-list/sw-customer-list.html.twig#L54) to see how the components and code works together.

![](https://res.cloudinary.com/dtgdh7noz/image/upload/v1607774042/Bildschirmfoto_2020-12-12_um_13.53.02_vfdvw9.png)
*Overview page dummy*

**There's no CMS element needed, just the entity with its admin module.**

Again: Always have a look at the Core, how Shopware is doing it.
If you're stuck or think you're going the wrong way: Ask 🗣️

#### Handling errors and required fields
You should always take a look at the Core, how Shopware is doing it.
Regarding the error handling within an admin component, take a look at the `mapPropertyErrors` computed property within any core admin component, for example the [sw-customer-address-form component](https://github.com/shopware/platform/blob/605efce89aa6be1566e842ed0582c4810f331c70/src/Administration/Resources/app/administration/src/module/sw-customer/component/sw-customer-address-form/index.js#L52-L67 )

If you take a look at the [map-error-serice](https://github.com/shopware/platform/blob/c5a981c9ca9ede2afe8eae5b0f0a6c861000b79c/src/Administration/Resources/app/administration/src/app/service/map-errors.service.js#L20) you will see that you have to pass your `entityName` followed by your properties which are representing the `propertyName` from your [entity definition](https://github.com/shopware/platform/blob/c4abfdc17d4583d3efd76498be395f5ec376828d/src/Core/Content/Product/Aggregate/ProductTranslation/ProductTranslationDefinition.php#L52-L59) within an array.

The `mapPropertyErrors` within your admin vue component are basically mapped to your Entity Definitions within the `FieldCollection`. You can have a look at the [Product Translation Entity](https://github.com/shopware/platform/blob/c4abfdc17d4583d3efd76498be395f5ec376828d/src/Core/Content/Product/Aggregate/ProductTranslation/ProductTranslationDefinition.php#L53) for example, where you pass the `new Required()` to the `addFlags` method. 

If you then have a look at the [sw-product-basic-form component](https://github.com/shopware/platform/blob/master/src/Administration/Resources/app/administration/src/module/sw-product/component/sw-product-basic-form/index.js#L29-L30) you will see that the **entityName** is passed - in this case `product`, followed by it's definitions as an array: In this case also the `name` [propertyName from the product translation defition](https://github.com/shopware/platform/blob/c4abfdc17d4583d3efd76498be395f5ec376828d/src/Core/Content/Product/Aggregate/ProductTranslation/ProductTranslationDefinition.php#L53).

#### Translations

Also make sure to use [translation snippets](https://docs.shopware.com/en/shopware-platform-dev-en/developer-guide/storefront/snippets) for labels, placeholders and text. 
You can again - Take a look at the Core code how it's done, for example within the [sw-customer-address-form component](https://github.com/shopware/platform/blob/605efce89aa6be1566e842ed0582c4810f331c70/src/Administration/Resources/app/administration/src/module/sw-customer/component/sw-customer-address-form/sw-customer-address-form.html.twig#L10)

### What we expect
Get the best out of you! Show what you are capable of. But don't do over engineering! Our goal is to see, if you're able to implement the requirements in a simple way but in high quality, which is good to maintain and improve in the future. We want to see your approach and see at which stage you are in your career currently to become an excellent developer.

A good developer code goes through 4 stages:
1. simple and wrong
2. complex and wrong
3. complex and right
4. simple and right

There is no real deadline for the task, but the sooner the better. But don't rush things, a high code quality is very important for us, therefore provide your best results - clean, lean high quality code!


### What we do
We will check your code, quality and collaborating with you.

###  Why we're going this way?
You can prove your skills and showing us your willingness, so we could compare your results with our code requirements. It is the quality that matters and we want to see your individual responsibility. I hope, this way is ok for you. If you have any questions, don't hesitate to contact me.

I know Shopware 6 is completly new for you and it may take some time to dive into it. So better ask if you don't know something instead of digging for hours and hours.

Looking forward hearing back from you.
As mentioned above, please use for further communication hello@shapeandshift.dev
