Forem Logo

DEV Community Logo

Gamers Forem Logo

Future Logo

Open Forem Logo

Music Forem Logo

Vibe Coding Forem Logo

Popcorn Movies and TV Logo

DUMB DEV Community Logo

Design Community Logo

Golf Forem Logo

Security Forem Logo

Scale Forem Logo

Forem Core Logo

Crypto Forem Logo

Parenting Logo

Maker Forem Logo

Skip to content

DEV Community

Search...

Powered by Algolia 

Log in

Create account

  

0

Jump to Comments

  

0

Save

  

Boost

  

Cover image for 🚀 Ensuring Unique Slugs in Next.js 15 with Prisma & Slugify

Saiful Islam

Saiful Islam

Posted on 16 Feb

  

🚀 Ensuring Unique Slugs in Next.js 15 with Prisma & Slugify

#

javascript

#

nextjs

#

react

#

prisma

Creating SEO-friendly slugs is essential for URLs in Next.js applications. However, ensuring they remain unique can be tricky—especially when dealing with dynamic content like blog posts.

  

In this guide, we’ll explore an efficient recursive approach to generate unique slugs using Prisma and slugify in Next.js 15! 🔥

  

🌟 Why Unique Slugs Matter?

✅ SEO Optimization – Clean, readable URLs improve search rankings

✅ User-Friendly Links – Easy to share & understand

✅ Prevents Collisions – Avoid duplicate slugs for different posts

  

🛠 Implementing Recursive Slug Generation

The following function:

✔ Converts a title into a slug

✔ Checks if the slug already exists in the database

✔ Recursively appends a suffix if the slug is taken

  

import db from "@/lib/db";

import slugify from "slugify";

  

/**

 * Generates a unique slug recursively.

 * @param input - The string to generate the slug from (e.g., a post title).

 * @param model - The Prisma model to check for existing slugs (e.g., "post").

 * @param suffix - Optional suffix to append for uniqueness (used internally in recursion).

 * @returns A unique slug.

 */

export default async function generateSlug(

  input: string,

  model: "post",

  suffix: number = 0,

): Promise<string> {

  const baseSlug = slugify(input, {

    lower: true,

    strict: true,

    trim: true,

  });

  

  const slug = suffix === 0 ? baseSlug : `${baseSlug}-${suffix}`;

  

  const existingRecord = await db[model].findUnique({

    where: { slug },

  });

  

  if (!existingRecord) {

    return slug;

  }

  

  return generateSlug(input, model, suffix + 1);

}

🔍 How This Works

1️⃣ Converts the input string into a lowercase, URL-friendly slug

2️⃣ Checks the database if a record with the same slug exists

3️⃣ If found, recursively calls itself with a numeric suffix (e.g., my-post, my-post-1, my-post-2...)

4️⃣ Returns the first available unique slug

  

📌 Example Usage in Next.js 15

You can use this function when creating new posts dynamically:

  

const createPost = async (title: string, content: string) => {

  const slug = await generateSlug(title, "post");

  

  await db.post.create({

    data: {

      title,

      slug,

      content,

    },

  });

  

  console.log(`✅ New post created with slug: ${slug}`);

};

✅ Calling createPost("My Next.js Guide", "Some content")

🚀 Generates my-nextjs-guide, or if taken, my-nextjs-guide-1, my-nextjs-guide-2, etc.

  

⚡ Key Benefits

  

🔥 Handles Duplicates Automatically – Ensures slugs remain unique

⚡ SEO & Readability – Clean and structured URLs

🛠 Works Seamlessly with Prisma – No extra logic needed in API routes

  

🚀 Final Thoughts

Slugs are a critical part of a Next.js app’s SEO and usability. With this recursive approach, you ensure that every post, page, or product gets a unique and clean URL, automatically.

  

💡 Would you use this approach in your project? Let’s discuss in the comments! 🚀

  

Top comments (0)

Subscribe

pic

Add to the discussion

Code of Conduct • Report abuse

DEV Community

  

Visualizing Promises and Async/Await 🤓

async await

  

☝️ Check out this all-time classic DEV post on visualizing Promises and Async/Await 🤓

  

  

Saiful Islam

Follow

I am Saiful Islam, a front-end developer skilled in React.js, TypeScript, JavaScript, and Tailwind CSS. I build user-friendly web apps and have worked on Pocket School Quiz, DTR CLI, and DevMingle.

Location

Lakshmipur, Bangladesh

Joined

8 Mar 2024

More from Saiful Islam

useSyncExternalStore in React — The Right Way to Subscribe to External Data

#javascript #react #typescript

How to Create an ESLint Config Package in Turborepo

#programming #webdev #javascript

Monorepo vs Polyrepo: Choosing the Right Structure for Your MERN Stack Project

#javascript #programming #node #typescript

💎 DEV Diamond Sponsors

  

Thank you to our Diamond Sponsors for supporting the DEV Community

  

Google AI - Official AI Model and Platform Partner

Google AI is the official AI Model and Platform Partner of DEV

  

Neon - Official Database Partner

Neon is the official database partner of DEV

  

Algolia - Official Search Partner

Algolia is the official search partner of DEV

  

DEV Community — A space to discuss and keep up software development and manage your software career

  

Home

DEV++

Reading List

Podcasts

Videos

DEV Education Tracks

DEV Challenges

DEV Help

Advertise on DEV

DEV Showcase

About

Contact

Free Postgres Database

Software comparisons

Forem Shop

Code of Conduct

Privacy Policy

Terms of Use

Built on Forem — the open source software that powers DEV and other inclusive communities.

  

Made with love and Ruby on Rails. DEV Community © 2016 - 2025.

  

simeonGriggs.dev

Twitter

GitHub

YouTube

  

Movable type slugs

Patterns for Next.js, Sanity.io, Catch-all Routes and Slugs

Hard coding document slugs in Sanity.io with validation rules can save you a lot of double-handling on both front and back end.

  

View in Sanity Studio

Slugs in Sanity

Benefits of this approach

Slugs in Next.js

Benefits of this approach

2021-04-19

I'm 100% sold on Next.js over Gatsby. It's not that Next.js is perfect, it just feels much more like that's where the puck is going. The speed and quality of releases just feels higher. And the feature set is broader.

  

I also feel like for anything outside of simple static pages Gatsby still will give you one experience in development and surprise you with random behaviour in production.

  

Sanity's already my CMS of choice. Even if Sanity "hate being compared to headless CMS", it's an excellent headless CMS.

  

With anything in programming there are so many different ways to achieve the same thing. So the patterns I'm outlining here are filed under "Works For Me". Not to be considered "Correct" or "Best Practice". Got thoughts? Let me hear them.

  

Slugs in Sanity

Using Sanity correctly is to model content types, not simply the pages of your website. However, the existence of a "Slug" field type infers a relationship between a Document in Sanity and a Page on a website.

  

Slugs are basically human-readable ID's. They should be unique. But there's not really a "Wrong" way to store them. Including adding / characters.

  

In its simplest form, your slug field is likely just a slugified version of the Document title. Depending on the structure of your website, you may then add prefixes to that slug, depending on the document _type.

  

With no extra configuration, your _type, title and slug field data will look something like this:

  

Copy

_type: `page`,

title: `Hello World`,

slug: { current: `hello-world` }

  

_type: `article`,

title: `Annual Report`,

slug: { current: `annual-report` }

These document's slugs aren't indicative of the final pathname on your website. So anywhere you plan to turn these into URLs, you'll run logic to prefix the document slug with the full URL on the website.

  

Copy

_type ===`article`?`/news/${slug.current}`:`/${slug.current}`;

But you can imagine how quickly this breaks down the more _types produce pages on the website. Or if you want the pathname to use something other than the _type. The number of times you re-run this logic quickly adds up too.

  

Instead, let's bake the pathname's prefixes into the slug.current. Because everything in Sanity is "Just JavaScript" that's as simple as some validation rules and a helper function. Here's how my documents's first few fields look:

  

Copy

fields: [

  { name: "title", type: "string" },

  slugWithType(`news`, `title`),

  // ...and so on

];

The slugWithType function takes two arguments. The prefix to put before the document's slug, and the field which the generate key will use.

  

Copy

import slugify from "slugify";

  

function formatSlug(input, slugStart) {

  const slug = slugify(input, { lower: true });

  return slugStart + slug;

}

  

export function slugWithType(prefix = ``, source = `title`) {

  const slugStart = prefix ? `/${prefix}/` : `/`;

  

  return {

    name: `slug`,

    type: `slug`,

    options: {

      source,

      slugify: (value) => formatSlug(value, slugStart),

    },

    validation: (Rule) =>

      Rule.required().custom(({ current }) => {

        if (typeof current === "undefined") {

          return true;

        }

  

        if (current) {

          if (!current.startsWith(slugStart)) {

            return `Slug must begin with "${slugStart}". Click "Generate" to reset.`;

          }

  

          if (current.slice(slugStart.length).split("").includes("/")) {

            return `Slug cannot have another "/" after "${slugStart}"`;

          }

  

          if (current === slugStart) {

            return `Slug cannot be empty`;

          }

  

          if (current.endsWith("/")) {

            return `Slug cannot end with "/"`;

          }

        }

  

        return true;

      }),

  };

}

Now our previous documents write slugs like this:

  

Copy

_type: `page`,

title: `Hello World`,

slug: { current: `/hello-world` }

  

_type: `article`,

title: `Annual Report`,

slug: { current: `/news/annual-report` }

Benefits of this approach

No more repeating logic to combine the _type with the slug, in the Document Preview, or in your resolveProductionUrl helper function, or anywhere else!

Reference fields are now actual links. No need to repeat that same logic again on the front end. Querying a document for its slug.current gives you the full path of the page.

Relationship between Documents and Web Pages are now explicit.

Sanity is now the single source of truth for all pathnames.

And, if your documents are being re-used for projects other than a single website, just add another slug field!

  

Slugs in Next.js

Next.js's file based routing is designed to give explicit control over what is displayed on any given path. And it may be tempting to create a folder for each schema type with prefixed pathnames, and a [slug].js file within each folder.

  

Copy

pages/index.js

pages/[slug].js

pages/news/index.js

pages/news/[slug].js

But for your average brochure website, this level of control is not worth the code duplication. Each one of these files likely needs to query for the same information. Header and Footer content, SEO details, etc. Then there's Sanity's usePreviewSubscription hook which is a whole lot more syntax to include in each file.

  

Instead, let's create every page on the website from one file:

  

Copy

pages/[[...slug]].js

This is a "Catch all route". And in it, depending on your particular schema, we can query every page and create every path, all from one query.

  

Copy

// pages/[[..slug]].js

  

export async function getStaticPaths() {

  const pageQueries = await getClient().fetch(

    groq`*[_type in ["homePage", "page", "article"] && defined(slug.current)][].slug.current`

  )

  

  // Split the slug strings to arrays (as required by Next.js)

  const paths = pageQueries.map((slug) => ({

    params: { slug: slug.split('/').filter((p) => p) },

  }))

  

  return { paths }

}

(Before changing all my documents slugs to have complete pathnames I had such a wonderful looking switch statement to cleverly loop over different types and object keys to create paths. Now, that's all gone!)

  

Because the only data we can send from getStaticPaths to getStaticProps is the slug array, this is the time where we'll need to do some detective work to match each slug with a GROQ query to fetch that page's required data.

  

Copy

// pages/[[..slug]].js

  

export async function getStaticProps({ params }) {

  const client = await getClient();

  

  // Every website has a bunch of global content that every page needs, too!

  const globalSettings = await client.fetch(globalSettingsQuery);

  

  // A helper function to work out what query we should run based on this slug

  const { query, queryParams, docType } = getQueryFromSlug(params.slug);

  

  // Get the initial data for this page, using the correct query

  const pageData = await client.fetch(query, queryParams);

  

  return {

    props: {

      data: { query, queryParams, docType, pageData, globalSettings },

    },

  };

}

The getQueryFromSlug function is where things get hairy, so I've only inserted a simplified version of it below. The more _type's you have, the more complex this function gets. But it should be the only place where you have to do this slug-array-to-query matching.

  

docQuery contains the queries we need to give each page its data. Because your actual GROQ queries are likely to be extended to include references and such, I'd store these in a separate file and import them as variables.

Switch over the slugArray and depending on its composition, send the right query to the document.

Send along a variable called docType to load the correct Component.

Copy

function getQueryFromSlug(slugArray = []) {

  const docQuery = {

    home: groq`*[_id == "homePage"][0]`,

    news: groq`*[_type == "article" && slug.current == $slug][0]`,

    page: groq`*[_type == "page" && slug.current == $slug][0]`,

  };

  

  if (slugArray.length === 0) {

    return {

      docType: "home",

      queryParams: {},

      query: docQuery.home,

    };

  }

  

  const [slugStart] = slugArray;

  

  // We now have to re-combine the slug array to match our slug in Sanity.

  let queryParams = { slug: `/${slugArray.join("/")}` };

  

  // Keep extending this section to match the slug against the docQuery object keys

  if (docQuery.hasOwnProperty(slugStart)) {

    docType = slugStart;

  } else {

    docType = `page`;

  }

  

  return {

    docType,

    queryParams,

    query: docQuery[docType],

  };

}

So now, we're:

  

Querying every Sanity document that will create a page

Finding the correct GROQ query for that page's data, based on each document's slug

Sending that query from getStaticProps to our component to render the page

The query is passed along from getStaticProps to the Page because of how Sanity's usePreviewSubscription needs to re-use it. And that is another whole blog post in itself!

  

Here's what our final Page component looks like (again, cut down to the bare basics for this demo). See how we re-use the query as passed-in from getStaticProps to populate the preview.

  

Then, depending on the docType set in our getQueryFromSlug function, we selectively import the correct Component for that page layout. It's important to note the dynamic() imports here. If the components were not dynamically imported, they would be automatically bundled into each Page. Blowing out our bundle size! (Which, too, is another whole blog post).

  

Copy

// pages/[[..slug]].js

import dynamic from "next/dynamic";

  

const PageSingle = dynamic(() => import("../components/layouts/PageSingle"));

const NewsSingle = dynamic(() => import("../components/layouts/NewsSingle"));

  

export default function Page({ data, preview }) {

  const { data: pageData } = usePreviewSubscription(data?.query, {

    params: data?.queryParams ?? {},

    initialData: data?.pageData,

    enabled: preview,

  });

  

  const { globalSettings, docType } = data;

  

  return (

    <Layout globalSettings={globalSettings}>

      <Seo meta={pageData.meta} />

      {docType === "home" && <PageSingle page={pageData} />}

      {docType === "page" && <PageSingle page={pageData} />}

      {docType === "news" && <NewsSingle post={pageData} />}

    </Layout>

  );

}

Benefits of this approach

Setting up our Global data, <Layout /> and Sanity Preview once in one file.

Keeping the relationship between Slugs and Page queries to one helper function.

There are potentially some drawbacks to this method of using catch-all routing. But I haven't found them yet.

  

There's more where this came from

Subscribe for updates. Not spam.

  

Your email address

Subscribe

  

  

Search

Write

Sign up

  

Sign in

  

  

  

Generating URL-Safe and Unique Slugs for Blog Posts in Next.js: A Comprehensive Guide

AGAMY

AGAMY

  

Follow

2 min read

·

Apr 12, 2024

3

  

  

1

  

  

  

Press enter or click to view image in full size

Generating URL-Safe and Unique Slugs for Blog Posts in Next.js

When building a blog or content management system with Next.js, creating SEO-friendly URLs is crucial for user experience and search engine visibility. One effective method is generating URL-safe “slugs” from blog post titles. This guide will walk you through creating a function to generate these slugs and integrating it into a Next.js application, while also touching on advanced SEO techniques to further optimize your blog’s visibility.

  

Prerequisites

Basic understanding of JavaScript and Next.js.

Next.js 14 or later installed in your project.

Creating the generateSlug Function

First, let’s define a function named generateSlug that transforms a blog post title into a URL-safe slug.

  

// utils/slug.js

  

export function generateSlug(title) {

  const slug = title

    .toLowerCase()             // Convert to lowercase

    .replace(/[^\w\s-]/g, '')  // Remove non-word characters except spaces and hyphens

    .trim()                    // Trim spaces

    .replace(/\s+/g, '-')      // Replace spaces with hyphens

    .slice(0, 50);             // Limit to 50 characters

  return slug;

}

Explanation

toLowerCase(): Ensures consistency by converting the title to lowercase.

replace(/[^\w\s-]/g, ''): Removes characters that are not word characters, spaces, or hyphens.

Get AGAMY’s stories in your inbox

Join Medium for free to get updates from this writer.

  

Enter your email

Subscribe

trim(): Removes leading or trailing spaces.

replace(/\s+/g, '-'): Replaces spaces with hyphens for URL-friendly formatting.

slice(0, 50): Limits the slug to 50 characters to avoid overly long URLs.

Integrating generateSlug in Next.js

Now, let’s see how to use generateSlug within a Next.js component for creating blog posts.

  

// pages/new-post.js

  

import { useState } from 'react';

import { generateSlug } from '../utils/slug';

  

const NewPost = () => {

  const [title, setTitle] = useState('');

  const [slug, setSlug] = useState('');

  const handleTitleChange = (e) => {

    const newTitle = e.target.value;

    setTitle(newTitle);

    const newSlug = generateSlug(newTitle);

    setSlug(newSlug);

  };

  const handleSubmit = (e) => {

    e.preventDefault();

    // Logic to create a new blog post with the title and slug

  };

  return (

    <div>

      <h1>Create a New Blog Post</h1>

      <form onSubmit={handleSubmit}>

        <label>

          Title:

          <input type='text' value={title} onChange={handleTitleChange} />

        </label>

        <button type='submit'>Submit</button>

      </form>

      {slug && (

        <p>

          URL Slug: <code>{slug}</code>

        </p>

      )}

    </div>

  );

};

export default NewPost;

Advanced SEO Techniques in Next.js

To further enhance your blog’s SEO, consider implementing the following advanced techniques:

  

Dynamic Meta Tags and Titles: Use server-side data fetching to dynamically generate meta tags and titles for each page.

Structured Data Markup: Implement structured data to provide search engines with detailed information about your content.

URL Structure and Canonical URLs: Ensure your URLs are well-structured and use canonical URLs to avoid duplicate content issues.

Sitemap Generation: Generate a sitemap to help search engines discover and index your pages.

Responsive and Mobile-Friendly Design: Ensure your blog is optimized for mobile devices to improve user experience and SEO.

Nextjs

Reactjs

JavaScript

Typescript

SEO

3

  

  

1

  

  

AGAMY

Written by AGAMY

0 followers

·

1 following

  

Follow

Responses (1)

  

Write a response

  

What are your thoughts?

  

Cancel

Respond

Ashirzuhaib

Ashirzuhaib

  

Nov 19, 2024

  

  

Thanks

4

  

Reply

  

Recommended from Medium

Next.js Image Optimization

Utsav Desai

Utsav Desai

  

Next.js Image Optimization: Ultimate Guide to next/image (2025 Edition)

Learn how to optimize images in Next.js using next/image for faster load times, better SEO, and Core Web Vitals. Boost performance with…

  

Jun 3

8

1

How to Configure MongoDB in the Cloud Using MongoDB Atlas for MERN Stack

JavaScript in Plain English

In

  

JavaScript in Plain English

  

by

  

codingsprints

  

How to Configure MongoDB in the Cloud Using MongoDB Atlas for MERN Stack

Learn step-by-step how to set up MongoDB Atlas, create a database, and connect it to your MERN Stack project in the cloud.

  

Nov 3

Stanford Just Killed Prompt Engineering With 8 Words (And I Can’t Believe It Worked)

Generative AI

In

  

Generative AI

  

by

  

Adham Khaled

  

Stanford Just Killed Prompt Engineering With 8 Words (And I Can’t Believe It Worked)

ChatGPT keeps giving you the same boring response? This new technique unlocks 2× more creativity from ANY AI model — no training required…

  

Oct 20

16.6K

380

📂 The Definitive Guide to Next.js

Matías Salinas

Matías Salinas

  

📂 The Definitive Guide to Next.js

A well-structured folder layout is the backbone of maintainable, scalable, and performant Next.js applications. Let’s dive deep into how to…

  

May 18

9

2

CSR and SSR

Sora

Sora

  

What are CSR and SSR

CSR renders pages in the browser with JS, while SSR generates HTML on the server. Each has unique pros, cons, and use cases.

5d ago

4

2

⚛️ Next.js 2025: The Ultimate Guide to Building High-Performance, AI-Driven, and Scalable Web Apps

Alisha ✨

Alisha ✨

  

⚛️ Next.js 2025: The Ultimate Guide to Building High-Performance, AI-Driven, and Scalable Web Apps

In the fast-moving world of web development, Next.js has become the heartbeat of modern full-stack web apps.  What started as a React…

  

Nov 3

44

See more recommendations

Help

  

Status

  

About

  

Careers

  

Press

  

Blog

  

Privacy

  

Rules

  

Terms

  

Text to speech

  

  

mongoose

Version 8.19.1

Search

  

Quick Start

Guides

Schemas

SchemaTypes

Connections

Models

Documents

Subdocuments

Queries

Validation

Middleware

Populate

Discriminators

Plugins

Timestamps

Transactions

TypeScript

API

Mongoose

Schema

Connection

Document

Model

Query

Aggregate

SchemaType

VirtualType

Migration Guide

Version Compatibility

Version Support

FAQ

Further Reading

For Enterprise

Sponsors

ads via CarbonAccelerate your DevOps journey with AWS's comprehensive toolkit.

ads via Carbon

  

Model

Model()

Model.$where()

Model.aggregate()

Model.applyDefaults()

Model.applyTimestamps()

Model.applyVirtuals()

Model.bulkSave()

Model.bulkWrite()

Model.castObject()

Model.cleanIndexes()

Model.clientEncryption()

Model.countDocuments()

Model.create()

Model.createCollection()

Model.createIndexes()

Model.createSearchIndex()

Model.createSearchIndexes()

Model.db

Model.deleteMany()

Model.deleteOne()

Model.diffIndexes()

Model.discriminator()

Model.distinct()

Model.dropSearchIndex()

Model.ensureIndexes()

Model.estimatedDocumentCount()

Model.events

Model.exists()

Model.find()

Model.findById()

Model.findByIdAndDelete()

Model.findByIdAndUpdate()

Model.findOne()

Model.findOneAndDelete()

Model.findOneAndReplace()

Model.findOneAndUpdate()

Model.hydrate()

Model.init()

Model.insertMany()

Model.insertOne()

Model.inspect()

Model.listIndexes()

Model.listSearchIndexes()

Model.namespace()

Model.populate()

Model.prototype.$model()

Model.prototype.$where

Model.prototype.base

Model.prototype.baseModelName

Model.prototype.collection

Model.prototype.collection

Model.prototype.db

Model.prototype.deleteOne()

Model.prototype.discriminators

Model.prototype.increment()

Model.prototype.model()

Model.prototype.modelName

Model.prototype.save()

Model.recompileSchema()

Model.replaceOne()

Model.schema

Model.startSession()

Model.syncIndexes()

Model.translateAliases()

Model.updateMany()

Model.updateOne()

Model.updateSearchIndex()

Model.useConnection()

Model.validate()

Model.watch()

Model.where()

Model()

Parameters:

doc «Object» values for initial set

[fields] «Object» optional object containing the fields that were selected in the query which returned this document. You do not need to set this parameter to ensure Mongoose handles your query projection.

[skipId=false] «Boolean» optional boolean. If true, mongoose doesn't add an _id field to the document.

Inherits:

«Document»

A Model is a class that's your primary tool for interacting with MongoDB. An instance of a Model is called a Document.

  

In Mongoose, the term "Model" refers to subclasses of the mongoose.Model class. You should not use the mongoose.Model class directly. The mongoose.model() and connection.model() functions create subclasses of mongoose.Model as shown below.

  

Example:

// `UserModel` is a "Model", a subclass of `mongoose.Model`.

const UserModel = mongoose.model('User', new Schema({ name: String }));

  

// You can use a Model to create new documents using `new`:

const userDoc = new UserModel({ name: 'Foo' });

await userDoc.save();

  

// You also use a model to create queries:

const userFromDb = await UserModel.findOne({ name: 'Foo' });

Model.$where()

Parameters:

argument «String|Function» is a javascript string or anonymous function

Returns:

«Query»

See:

Query.$where

Creates a Query and specifies a $where condition.

  

Sometimes you need to query for things in mongodb using a JavaScript expression. You can do so via find({ $where: javascript }), or you can use the mongoose shortcut method $where via a Query chain or from your mongoose Model.

  

Blog.$where('this.username.indexOf("val") !== -1').exec(function (err, docs) {});

Model.aggregate()

Parameters:

[pipeline] «Array» aggregation pipeline as an array of objects

[options] «Object» aggregation options

Returns:

«Aggregate»

See:

Aggregate

MongoDB

Performs aggregations on the models collection.

  

The aggregate itself is returned.

  

This function triggers the following middleware.

  

aggregate()

Example:

// Find the max balance of all accounts

const res = await Users.aggregate([

  { $group: { _id: null, maxBalance: { $max: '$balance' }}},

  { $project: { _id: 0, maxBalance: 1 }}

]);

  

console.log(res); // [ { maxBalance: 98000 } ]

  

// Or use the aggregation pipeline builder.

const res = await Users.aggregate().

  group({ _id: null, maxBalance: { $max: '$balance' } }).

  project('-id maxBalance').

  exec();

console.log(res); // [ { maxBalance: 98 } ]

Note:

Mongoose does not cast aggregation pipelines to the model's schema because $project and $group operators allow redefining the "shape" of the documents at any stage of the pipeline, which may leave documents in an incompatible format. You can use the mongoose-cast-aggregation plugin to enable minimal casting for aggregation pipelines.

The documents returned are plain javascript objects, not mongoose documents (since any shape of document can be returned).

More About Aggregations:

Mongoose Aggregate

An Introduction to Mongoose Aggregate

MongoDB Aggregation docs

Model.applyDefaults()

Parameters:

obj «Object|Document» object or document to apply defaults on

Apply defaults to the given document or POJO.

  

Model.applyTimestamps()

Parameters:

obj «Object» object or document to apply virtuals on

[options] «Object»

[options.isUpdate=false] «Boolean» if true, treat this as an update: just set updatedAt, skip setting createdAt. If false, set both createdAt and updatedAt

[options.currentTime] «Function» if set, Mongoose will call this function to get the current time.

Apply this model's timestamps to a given POJO, including subdocument timestamps

  

Example:

const userSchema = new Schema({ name: String }, { timestamps: true });

const User = mongoose.model('User', userSchema);

  

const obj = { name: 'John' };

User.applyTimestamps(obj);

obj.createdAt; // 2024-06-01T18:00:00.000Z

obj.updatedAt; // 2024-06-01T18:00:00.000Z

Model.applyVirtuals()

Parameters:

obj «Object» object or document to apply virtuals on

[virtualsToApply] «Array<string>» optional whitelist of virtuals to apply

Apply this model's virtuals to a given POJO. Virtuals execute with the POJO as the context this.

  

Example:

const userSchema = new Schema({ name: String });

userSchema.virtual('upper').get(function() { return this.name.toUpperCase(); });

const User = mongoose.model('User', userSchema);

  

const obj = { name: 'John' };

User.applyVirtuals(obj);

obj.name; // 'John'

obj.upper; // 'JOHN', Mongoose applied the return value of the virtual to the given object

Model.bulkSave()

Parameters:

documents «Array<Document>»

[options] «Object» options passed to the underlying bulkWrite()

[options.timestamps] «Boolean» defaults to null, when set to false, mongoose will not add/update timestamps to the documents.

[options.session=null] «ClientSession» The session associated with this bulk write. See transactions docs.

[options.w=1] «String|number» The write concern. See Query#w() for more information.

[options.wtimeout=null] «number» The write concern timeout.

[options.j=true] «Boolean» If false, disable journal acknowledgement

[options.validateBeforeSave=true] «Boolean» set to false to skip Mongoose validation on all documents

Returns:

«BulkWriteResult» the return value from bulkWrite()

Takes an array of documents, gets the changes and inserts/updates documents in the database according to whether or not the document is new, or whether it has changes or not.

  

bulkSave uses bulkWrite under the hood, so it's mostly useful when dealing with many documents (10K+)

  

bulkSave() throws errors under the following conditions:

  

one of the provided documents fails validation. In this case, bulkSave() does not send a bulkWrite(), and throws the first validation error.

bulkWrite() fails (for example, due to being unable to connect to MongoDB or due to duplicate key error)

bulkWrite() did not insert or update any documents. In this case, bulkSave() will throw a DocumentNotFound error.

Note that bulkSave() will not throw an error if only some of the save() calls succeeded.

  

Model.bulkWrite()

Parameters:

ops «Array»

[ops.insertOne.document] «Object» The document to insert

[ops.insertOne.timestamps=true] «Object» If false, do not apply timestamps to the operation

[ops.updateOne.filter] «Object» Update the first document that matches this filter

[ops.updateOne.update] «Object» An object containing update operators

[ops.updateOne.upsert=false] «Boolean» If true, insert a doc if none match

[ops.updateOne.timestamps=true] «Boolean» If false, do not apply timestamps to the operation

[ops.updateOne.collation] «Object» The MongoDB collation to use

[ops.updateOne.arrayFilters] «Array» The array filters used in update

[ops.updateMany.filter] «Object» Update all the documents that match this filter

[ops.updateMany.update] «Object» An object containing update operators

[ops.updateMany.upsert=false] «Boolean» If true, insert a doc if no documents match filter

[ops.updateMany.timestamps=true] «Boolean» If false, do not apply timestamps to the operation

[ops.updateMany.collation] «Object» The MongoDB collation to use

[ops.updateMany.arrayFilters] «Array» The array filters used in update

[ops.deleteOne.filter] «Object» Delete the first document that matches this filter

[ops.deleteMany.filter] «Object» Delete all documents that match this filter

[ops.replaceOne.filter] «Object» Replace the first document that matches this filter

[ops.replaceOne.replacement] «Object» The replacement document

[ops.replaceOne.upsert=false] «Boolean» If true, insert a doc if no documents match filter

[ops.replaceOne.timestamps=true] «Object» If false, do not apply timestamps to the operation

[options] «Object»

[options.ordered=true] «Boolean» If true, execute writes in order and stop at the first error. If false, execute writes in parallel and continue until all writes have either succeeded or errored.

[options.timestamps=true] «Boolean» If false, do not apply timestamps to any operations. Can be overridden at the operation-level.

[options.session=null] «ClientSession» The session associated with this bulk write. See transactions docs.

[options.w=1] «String|number» The write concern. See Query#w() for more information.

[options.wtimeout=null] «number» The write concern timeout.

[options.j=true] «Boolean» If false, disable journal acknowledgement

[options.skipValidation=false] «Boolean» Set to true to skip Mongoose schema validation on bulk write operations. Mongoose currently runs validation on insertOne and replaceOne operations by default.

[options.bypassDocumentValidation=false] «Boolean» If true, disable MongoDB server-side schema validation for all writes in this bulk.

[options.throwOnValidationError=false] «Boolean» If true and ordered: false, throw an error if one of the operations failed validation, but all valid operations completed successfully. Note that Mongoose will still send all valid operations to the MongoDB server.

[options.strict=null] «Boolean|[object Object]» Overwrites the strict option on schema. If false, allows filtering and writing fields not defined in the schema for all writes in this bulk.

Returns:

«Promise» resolves to a BulkWriteOpResult if the operation succeeds

Sends multiple insertOne, updateOne, updateMany, replaceOne, deleteOne, and/or deleteMany operations to the MongoDB server in one command. This is faster than sending multiple independent operations (e.g. if you use create()) because with bulkWrite() there is only one round trip to MongoDB.

  

Mongoose will perform casting on all operations you provide. The only exception is setting the update operator for updateOne or updateMany to a pipeline: Mongoose does not cast update pipelines.

  

This function does not trigger any middleware, neither save(), nor update(). If you need to trigger save() middleware for every document use create() instead.

  

Example:

Character.bulkWrite([

  {

    insertOne: {

      document: {

        name: 'Eddard Stark',

        title: 'Warden of the North'

      }

    }

  },

  {

    updateOne: {

      filter: { name: 'Eddard Stark' },

      // If you were using the MongoDB driver directly, you'd need to do

      // `update: { $set: { title: ... } }` but mongoose adds $set for

      // you.

      update: { title: 'Hand of the King' }

    }

  },

  {

    deleteOne: {

      filter: { name: 'Eddard Stark' }

    }

  }

]).then(res => {

 // Prints "1 1 1"

 console.log(res.insertedCount, res.modifiedCount, res.deletedCount);

});

  

// Mongoose does **not** cast update pipelines, so no casting for the `update` option below.

// Mongoose does still cast `filter`

await Character.bulkWrite([{

  updateOne: {

    filter: { name: 'Annika Hansen' },

    update: [{ $set: { name: 7 } }] // Array means update pipeline, so Mongoose skips casting

  }

}]);

The supported operations are:

  

insertOne

updateOne

updateMany

deleteOne

deleteMany

replaceOne

Model.castObject()

Parameters:

obj «Object» object or document to cast

options «Object» options passed to castObject

options.ignoreCastErrors «Boolean» If set to true will not throw a ValidationError and only return values that were successfully cast.

Cast the given POJO to the model's schema

  

Example:

const Test = mongoose.model('Test', Schema({ num: Number }));

  

const obj = Test.castObject({ num: '42' });

obj.num; // 42 as a number

  

Test.castObject({ num: 'not a number' }); // Throws a ValidationError

Model.cleanIndexes()

Parameters:

[options] «Object»

[options.toDrop] «Array<String>» if specified, contains a list of index names to drop

[options.hideIndexes=false] «Boolean» set to true to hide indexes instead of dropping. Requires MongoDB server 4.4 or higher

Returns:

«Promise<Array<String>>» list of dropped or hidden index names

Deletes all indexes that aren't defined in this model's schema. Used by syncIndexes().

  

The returned promise resolves to a list of the dropped indexes' names as an array

  

Model.clientEncryption()

If auto encryption is enabled, returns a ClientEncryption instance that is configured with the same settings that Mongoose's underlying MongoClient is using. If the client has not yet been configured, returns null.

  

Model.countDocuments()

Parameters:

filter «Object»

Returns:

«Query»

Counts number of documents matching filter in a database collection.

  

Example:

const count = await Adventure.countDocuments({ type: 'jungle' });

console.log('there are %d jungle adventures', count);

If you want to count all documents in a large collection, use the estimatedDocumentCount() function instead. If you call countDocuments({}), MongoDB will always execute a full collection scan and not use any indexes.

  

The countDocuments() function is similar to count(), but there are a few operators that countDocuments() does not support. Below are the operators that count() supports but countDocuments() does not, and the suggested replacement:

  

$where: $expr

$near: $geoWithin with $center

$nearSphere: $geoWithin with $centerSphere

Model.create()

Parameters:

docs «Array|Object» Documents to insert, as a spread or array

[options] «Object» Options passed down to save(). To specify options, docs must be an array, not a spread. See Model.save for available options.

[options.ordered] «Boolean» saves the docs in series rather than parallel.

[options.aggregateErrors] «Boolean» Aggregate Errors instead of throwing the first one that occurs. Default: false

Returns:

«Promise»

Shortcut for saving one or more documents to the database. MyModel.create(docs) does new MyModel(doc).save() for every doc in docs.

  

This function triggers the following middleware.

  

save()

Example:

// Insert one new `Character` document

await Character.create({ name: 'Jean-Luc Picard' });

  

// Insert multiple new `Character` documents

await Character.create([{ name: 'Will Riker' }, { name: 'Geordi LaForge' }]);

  

// Create a new character within a transaction. Note that you **must**

// pass an array as the first parameter to `create()` if you want to

// specify options.

await Character.create([{ name: 'Jean-Luc Picard' }], { session });

Model.createCollection()

Parameters:

[options] «Object» see MongoDB driver docs

Create the collection for this model. By default, if no indexes are specified, mongoose will not create the collection for the model until any documents are created. Use this method to create the collection explicitly.

  

Note 1: You may need to call this before starting a transaction See https://www.mongodb.com/docs/manual/core/transactions/#transactions-and-operations

  

Note 2: You don't have to call this if your schema contains index or unique field. In that case, just use Model.init()

  

Example:

const userSchema = new Schema({ name: String })

const User = mongoose.model('User', userSchema);

  

User.createCollection().then(function(collection) {

  console.log('Collection is created!');

});

Model.createIndexes()

Parameters:

[options] «Object» internal options

Returns:

«Promise»

Similar to ensureIndexes(), except for it uses the createIndex function.

  

Model.createSearchIndex()

Parameters:

description «Object» index options, including name and definition

description.name «String»

description.definition «Object»

Returns:

«Promise»

Create an Atlas search index. This function only works when connected to MongoDB Atlas.

  

Example:

const schema = new Schema({ name: { type: String, unique: true } });

const Customer = mongoose.model('Customer', schema);

await Customer.createSearchIndex({ name: 'test', definition: { mappings: { dynamic: true } } });

Model.createSearchIndexes()

Returns:

«Promise» resolves to the results of creating the search indexes

Creates all Atlas search indexes defined in this model's schema. This function only works when connected to MongoDB Atlas.

  

Example:

const schema = new Schema({

  name: String,

  description: String

});

schema.searchIndex({ name: 'test', definition: { mappings: { dynamic: true } } });

const Product = mongoose.model('Product', schema);

  

// Creates the search index defined in the schema

await Product.createSearchIndexes();

Model.db

Type:

«property»

Connection instance the model uses.

  

Model.deleteMany()

Parameters:

conditions «Object»

[options] «Object» optional see Query.prototype.setOptions()

[options.translateAliases=null] «Boolean» If set to true, translates any schema-defined aliases in filter, projection, update, and distinct. Throws an error if there are any conflicts where both alias and raw property are defined on the same object.

Returns:

«Query»

Deletes all of the documents that match conditions from the collection. It returns an object with the property deletedCount containing the number of documents deleted.

  

Example:

await Character.deleteMany({ name: /Stark/, age: { $gte: 18 } }); // returns {deletedCount: x} where x is the number of documents deleted.

Note:

This function triggers deleteMany query hooks. Read the middleware docs to learn more.

  

Model.deleteOne()

Parameters:

conditions «Object»

[options] «Object» optional see Query.prototype.setOptions()

[options.translateAliases=null] «Boolean» If set to true, translates any schema-defined aliases in filter, projection, update, and distinct. Throws an error if there are any conflicts where both alias and raw property are defined on the same object.

Returns:

«Query»

Deletes the first document that matches conditions from the collection. It returns an object with the property deletedCount indicating how many documents were deleted.

  

Example:

await Character.deleteOne({ name: 'Eddard Stark' }); // returns {deletedCount: 1}

Note:

This function triggers deleteOne query hooks. Read the middleware docs to learn more.

  

Model.diffIndexes()

Parameters:

[options] «Object»

[options.indexOptionsToCreate=false] «Boolean» If true, toCreate will include both the index spec and the index options, not just the index spec

Returns:

«Promise<Object>» contains the indexes that would be dropped in MongoDB and indexes that would be created in MongoDB as { toDrop: string[], toCreate: string[] }.

Does a dry-run of Model.syncIndexes(), returning the indexes that syncIndexes() would drop and create if you were to run syncIndexes().

  

Example:

const { toDrop, toCreate } = await Model.diffIndexes();

toDrop; // Array of strings containing names of indexes that `syncIndexes()` will drop

toCreate; // Array of index specs containing the keys of indexes that `syncIndexes()` will create

Model.discriminator()

Parameters:

name «String» discriminator model name

schema «Schema» discriminator model schema

[options] «Object|String» If string, same as options.value.

[options.value] «String» the string stored in the discriminatorKey property. If not specified, Mongoose uses the name parameter.

[options.clone=true] «Boolean» By default, discriminator() clones the given schema. Set to false to skip cloning.

[options.overwriteModels=false] «Boolean» by default, Mongoose does not allow you to define a discriminator with the same name as another discriminator. Set this to allow overwriting discriminators with the same name.

[options.mergeHooks=true] «Boolean» By default, Mongoose merges the base schema's hooks with the discriminator schema's hooks. Set this option to false to make Mongoose use the discriminator schema's hooks instead.

[options.mergePlugins=true] «Boolean» By default, Mongoose merges the base schema's plugins with the discriminator schema's plugins. Set this option to false to make Mongoose use the discriminator schema's plugins instead.

Returns:

«Model» The newly created discriminator model

Adds a discriminator type.

  

Example:

function BaseSchema() {

  Schema.apply(this, arguments);

  

  this.add({

    name: String,

    createdAt: Date

  });

}

util.inherits(BaseSchema, Schema);

  

const PersonSchema = new BaseSchema();

const BossSchema = new BaseSchema({ department: String });

  

const Person = mongoose.model('Person', PersonSchema);

const Boss = Person.discriminator('Boss', BossSchema);

new Boss().__t; // "Boss". `__t` is the default `discriminatorKey`

  

const employeeSchema = new Schema({ boss: ObjectId });

const Employee = Person.discriminator('Employee', employeeSchema, 'staff');

new Employee().__t; // "staff" because of 3rd argument above

Model.distinct()

Parameters:

field «String»

[conditions] «Object» optional

[options] «Object» optional

Returns:

«Query»

Creates a Query for a distinct operation.

  

Example:

const query = Link.distinct('url');

query.exec();

Model.dropSearchIndex()

Parameters:

name «String»

Returns:

«Promise»

Delete an existing Atlas search index by name. This function only works when connected to MongoDB Atlas.

  

Example:

const schema = new Schema({ name: { type: String, unique: true } });

const Customer = mongoose.model('Customer', schema);

await Customer.dropSearchIndex('test');

Model.ensureIndexes()

Parameters:

[options] «Object» internal options

Returns:

«Promise»

Sends createIndex commands to mongo for each index declared in the schema. The createIndex commands are sent in series.

  

Example:

await Event.ensureIndexes();

After completion, an index event is emitted on this Model passing an error if one occurred.

  

Example:

const eventSchema = new Schema({ thing: { type: 'string', unique: true } })

const Event = mongoose.model('Event', eventSchema);

  

Event.on('index', function (err) {

  if (err) console.error(err); // error occurred during index creation

});

NOTE: It is not recommended that you run this in production. Index creation may impact database performance depending on your load. Use with caution.

  

Model.estimatedDocumentCount()

Parameters:

[options] «Object»

Returns:

«Query»

Estimates the number of documents in the MongoDB collection. Faster than using countDocuments() for large collections because estimatedDocumentCount() uses collection metadata rather than scanning the entire collection.

  

Example:

const numAdventures = await Adventure.estimatedDocumentCount();

Model.events

Type:

«property»

Event emitter that reports any errors that occurred. Useful for global error handling.

  

Example:

MyModel.events.on('error', err => console.log(err.message));

  

// Prints a 'CastError' because of the above handler

await MyModel.findOne({ _id: 'Not a valid ObjectId' }).catch(noop);

Model.exists()

Parameters:

filter «Object»

[options] «Object» optional see Query.prototype.setOptions()

Returns:

«Query»

Returns a document with _id only if at least one document exists in the database that matches the given filter, and null otherwise.

  

Under the hood, MyModel.exists({ answer: 42 }) is equivalent to MyModel.findOne({ answer: 42 }).select({ _id: 1 }).lean()

  

Example:

await Character.deleteMany({});

await Character.create({ name: 'Jean-Luc Picard' });

  

await Character.exists({ name: /picard/i }); // { _id: ... }

await Character.exists({ name: /riker/i }); // null

This function triggers the following middleware.

  

findOne()

Model.find()

Parameters:

filter «Object|ObjectId»

[projection] «Object|String|Array[String]» optional fields to return, see Query.prototype.select()

[options] «Object» optional see Query.prototype.setOptions()

[options.translateAliases=null] «Boolean» If set to true, translates any schema-defined aliases in filter, projection, update, and distinct. Throws an error if there are any conflicts where both alias and raw property are defined on the same object.

Returns:

«Query»

See:

field selection

query casting

Finds documents.

  

Mongoose casts the filter to match the model's schema before the command is sent. See our query casting tutorial for more information on how Mongoose casts filter.

  

Example:

// find all documents

await MyModel.find({});

  

// find all documents named john and at least 18

await MyModel.find({ name: 'john', age: { $gte: 18 } }).exec();

  

// executes, name LIKE john and only selecting the "name" and "friends" fields

await MyModel.find({ name: /john/i }, 'name friends').exec();

  

// passing options

await MyModel.find({ name: /john/i }, null, { skip: 10 }).exec();

Model.findById()

Parameters:

id «Any» value of _id to query by

[projection] «Object|String|Array[String]» optional fields to return, see Query.prototype.select()

[options] «Object» optional see Query.prototype.setOptions()

Returns:

«Query»

See:

field selection

lean queries

findById in Mongoose

Finds a single document by its _id field. findById(id) is equivalent to findOne({ _id: id }).

  

The id is cast based on the Schema before sending the command.

  

This function triggers the following middleware.

  

findOne()

Example:

// Find the adventure with the given `id`, or `null` if not found

await Adventure.findById(id).exec();

  

// select only the adventures name and length

await Adventure.findById(id, 'name length').exec();

Model.findByIdAndDelete()

Parameters:

id «Object|Number|String» value of _id to query by

[options] «Object» optional see Query.prototype.setOptions()

[options.strict] «Boolean|String» overwrites the schema's strict mode option

[options.translateAliases=null] «Boolean» If set to true, translates any schema-defined aliases in filter, projection, update, and distinct. Throws an error if there are any conflicts where both alias and raw property are defined on the same object.

Returns:

«Query»

See:

Model.findOneAndDelete

mongodb

Issue a MongoDB findOneAndDelete() command by a document's _id field. In other words, findByIdAndDelete(id) is a shorthand for findOneAndDelete({ _id: id }).

  

This function triggers the following middleware.

  

findOneAndDelete()

Model.findByIdAndUpdate()

Parameters:

id «Object|Number|String» value of _id to query by

[update] «Object»

[options] «Object» optional see Query.prototype.setOptions()

[options.returnDocument='before'] «String» Has two possible values, 'before' and 'after'. By default, it will return the document before the update was applied.

[options.lean] «Object» if truthy, mongoose will return the document as a plain JavaScript object rather than a mongoose document. See Query.lean() and the Mongoose lean tutorial.

[options.session=null] «ClientSession» The session associated with this query. See transactions docs.

[options.strict] «Boolean|String» overwrites the schema's strict mode option

[options.timestamps=null] «Boolean» If set to false and schema-level timestamps are enabled, skip timestamps for this update. Note that this allows you to overwrite timestamps. Does nothing if schema-level timestamps are not set.

[options.sort] «Object|String» if multiple docs are found by the conditions, sets the sort order to choose which doc to update.

[options.runValidators] «Boolean» if true, runs update validators on this command. Update validators validate the update operation against the model's schema

[options.setDefaultsOnInsert=true] «Boolean» If setDefaultsOnInsert and upsert are true, mongoose will apply the defaults specified in the model's schema if a new document is created

[options.includeResultMetadata] «Boolean» if true, returns the full ModifyResult from the MongoDB driver rather than just the document

[options.upsert=false] «Boolean» if true, and no documents found, insert a new document

[options.new=false] «Boolean» if true, return the modified document rather than the original

[options.select] «Object|String» sets the document fields to return.

[options.translateAliases=null] «Boolean» If set to true, translates any schema-defined aliases in filter, projection, update, and distinct. Throws an error if there are any conflicts where both alias and raw property are defined on the same object.

[options.overwriteDiscriminatorKey=false] «Boolean» Mongoose removes discriminator key updates from update by default, set overwriteDiscriminatorKey to true to allow updating the discriminator key

Returns:

«Query»

See:

Model.findOneAndUpdate

mongodb

Issues a mongodb findOneAndUpdate command by a document's _id field. findByIdAndUpdate(id, ...) is equivalent to findOneAndUpdate({ _id: id }, ...).

  

Finds a matching document, updates it according to the update arg, passing any options, and returns the found document (if any).

  

This function triggers the following middleware.

  

findOneAndUpdate()

Example:

A.findByIdAndUpdate(id, update, options)  // returns Query

A.findByIdAndUpdate(id, update)           // returns Query

A.findByIdAndUpdate()                     // returns Query

Note:

All top level update keys which are not atomic operation names are treated as set operations:

  

Example:

Model.findByIdAndUpdate(id, { name: 'jason bourne' }, options)

  

// is sent as

Model.findByIdAndUpdate(id, { $set: { name: 'jason bourne' }}, options)

Note:

findOneAndX and findByIdAndX functions support limited validation. You can enable validation by setting the runValidators option.

  

If you need full-fledged validation, use the traditional approach of first retrieving the document.

  

const doc = await Model.findById(id)

doc.name = 'jason bourne';

await doc.save();

Model.findOne()

Parameters:

[conditions] «Object»

[projection] «Object|String|Array[String]» optional fields to return, see Query.prototype.select()

[options] «Object» optional see Query.prototype.setOptions()

[options.translateAliases=null] «Boolean» If set to true, translates any schema-defined aliases in filter, projection, update, and distinct. Throws an error if there are any conflicts where both alias and raw property are defined on the same object.

Returns:

«Query»

See:

field selection

lean queries

Finds one document.

  

The conditions are cast to their respective SchemaTypes before the command is sent.

  

Note: conditions is optional, and if conditions is null or undefined, mongoose will send an empty findOne command to MongoDB, which will return an arbitrary document. If you're querying by _id, use findById() instead.

  

Example:

// Find one adventure whose `country` is 'Croatia', otherwise `null`

await Adventure.findOne({ country: 'Croatia' }).exec();

  

// Model.findOne() no longer accepts a callback

  

// Select only the adventures name and length

await Adventure.findOne({ country: 'Croatia' }, 'name length').exec();

Model.findOneAndDelete()

Parameters:

conditions «Object»

[options] «Object» optional see Query.prototype.setOptions()

[options.strict] «Boolean|String» overwrites the schema's strict mode option

[options.projection=null] «Object|String|Array[String]» optional fields to return, see Query.prototype.select()

[options.session=null] «ClientSession» The session associated with this query. See transactions docs.

[options.includeResultMetadata] «Boolean» if true, returns the full ModifyResult from the MongoDB driver rather than just the document

[options.sort] «Object|String» if multiple docs are found by the conditions, sets the sort order to choose which doc to update.

[options.select] «Object|String» sets the document fields to return.

[options.maxTimeMS] «Number» puts a time limit on the query - requires mongodb >= 2.6.0

[options.translateAliases=null] «Boolean» If set to true, translates any schema-defined aliases in filter, projection, update, and distinct. Throws an error if there are any conflicts where both alias and raw property are defined on the same object.

Returns:

«Query»

Issue a MongoDB findOneAndDelete() command.

  

Finds a matching document, removes it, and returns the found document (if any).

  

This function triggers the following middleware.

  

findOneAndDelete()

Example:

A.findOneAndDelete(conditions, options)  // return Query

A.findOneAndDelete(conditions) // returns Query

A.findOneAndDelete()           // returns Query

findOneAndX and findByIdAndX functions support limited validation. You can enable validation by setting the runValidators option.

  

If you need full-fledged validation, use the traditional approach of first retrieving the document.

  

const doc = await Model.findById(id)

doc.name = 'jason bourne';

await doc.save();

Model.findOneAndReplace()

Parameters:

filter «Object» Replace the first document that matches this filter

[replacement] «Object» Replace with this document

[options] «Object» optional see Query.prototype.setOptions()

[options.returnDocument='before'] «String» Has two possible values, 'before' and 'after'. By default, it will return the document before the update was applied.

[options.lean] «Object» if truthy, mongoose will return the document as a plain JavaScript object rather than a mongoose document. See Query.lean() and the Mongoose lean tutorial.

[options.session=null] «ClientSession» The session associated with this query. See transactions docs.

[options.strict] «Boolean|String» overwrites the schema's strict mode option

[options.timestamps=null] «Boolean» If set to false and schema-level timestamps are enabled, skip timestamps for this update. Note that this allows you to overwrite timestamps. Does nothing if schema-level timestamps are not set.

[options.projection=null] «Object|String|Array[String]» optional fields to return, see Query.prototype.select()

[options.sort] «Object|String» if multiple docs are found by the conditions, sets the sort order to choose which doc to update.

[options.includeResultMetadata] «Boolean» if true, returns the full ModifyResult from the MongoDB driver rather than just the document

[options.select] «Object|String» sets the document fields to return.

[options.maxTimeMS] «Number» puts a time limit on the query - requires mongodb >= 2.6.0

[options.translateAliases=null] «Boolean» If set to true, translates any schema-defined aliases in filter, projection, update, and distinct. Throws an error if there are any conflicts where both alias and raw property are defined on the same object.

Returns:

«Query»

Issue a MongoDB findOneAndReplace() command.

  

Finds a matching document, replaces it with the provided doc, and returns the document.

  

This function triggers the following query middleware.

  

findOneAndReplace()

Example:

A.findOneAndReplace(filter, replacement, options)  // return Query

A.findOneAndReplace(filter, replacement) // returns Query

A.findOneAndReplace()                    // returns Query

Model.findOneAndUpdate()

Parameters:

[conditions] «Object»

[update] «Object»

[options] «Object» optional see Query.prototype.setOptions()

[options.returnDocument='before'] «String» Has two possible values, 'before' and 'after'. By default, it will return the document before the update was applied.

[options.lean] «Object» if truthy, mongoose will return the document as a plain JavaScript object rather than a mongoose document. See Query.lean() and the Mongoose lean tutorial.

[options.session=null] «ClientSession» The session associated with this query. See transactions docs.

[options.strict] «Boolean|String» overwrites the schema's strict mode option

[options.timestamps=null] «Boolean» If set to false and schema-level timestamps are enabled, skip timestamps for this update. Note that this allows you to overwrite timestamps. Does nothing if schema-level timestamps are not set.

[options.upsert=false] «Boolean» if true, and no documents found, insert a new document

[options.projection=null] «Object|String|Array[String]» optional fields to return, see Query.prototype.select()

[options.new=false] «Boolean» if true, return the modified document rather than the original

[options.fields] «Object|String» Field selection. Equivalent to .select(fields).findOneAndUpdate()

[options.maxTimeMS] «Number» puts a time limit on the query - requires mongodb >= 2.6.0

[options.sort] «Object|String» if multiple docs are found by the conditions, sets the sort order to choose which doc to update.

[options.runValidators] «Boolean» if true, runs update validators on this command. Update validators validate the update operation against the model's schema

[options.setDefaultsOnInsert=true] «Boolean» If setDefaultsOnInsert and upsert are true, mongoose will apply the defaults specified in the model's schema if a new document is created

[options.includeResultMetadata] «Boolean» if true, returns the raw result from the MongoDB driver

[options.translateAliases=null] «Boolean» If set to true, translates any schema-defined aliases in filter, projection, update, and distinct. Throws an error if there are any conflicts where both alias and raw property are defined on the same object.

[options.overwriteDiscriminatorKey=false] «Boolean» Mongoose removes discriminator key updates from update by default, set overwriteDiscriminatorKey to true to allow updating the discriminator key

Returns:

«Query»

See:

Tutorial

mongodb

Issues a mongodb findOneAndUpdate command.

  

Finds a matching document, updates it according to the update arg, passing any options. A Query object is returned.

  

Example:

A.findOneAndUpdate(filter, update, options);  // returns Query

A.findOneAndUpdate(filter, update);           // returns Query

A.findOneAndUpdate(filter);                   // returns Query

A.findOneAndUpdate();                         // returns Query

  

// Other supported syntaxes

// Note that calling `Query#findOneAndUpdate()` with 1 arg will treat the arg as `update`, NOT `filter`

A.find(filter).findOneAndUpdate(update);

Note:

All top level update keys which are not atomic operation names are treated as set operations:

  

Example:

const query = { name: 'borne' };

Model.findOneAndUpdate(query, { name: 'jason bourne' }, options);

  

// is sent as

Model.findOneAndUpdate(query, { $set: { name: 'jason bourne' }}, options);

Note:

findOneAndX and findByIdAndX functions support limited validation that you can enable by setting the runValidators option.

  

If you need full-fledged validation, use the traditional approach of first retrieving the document.

  

const doc = await Model.findById(id);

doc.name = 'jason bourne';

await doc.save();

Model.hydrate()

Parameters:

obj «Object»

[projection] «Object|String|Array[String]» optional projection containing which fields should be selected for this document

[options] «Object» optional options

[options.setters=false] «Boolean» if true, apply schema setters when hydrating

[options.hydratedPopulatedDocs=false] «Boolean» if true, populates the docs if passing pre-populated data

[options.virtuals=false] «Boolean» if true, sets any virtuals present on obj

Returns:

«Document» document instance

Shortcut for creating a new Document from existing raw data, pre-saved in the DB. The document returned has no paths marked as modified initially.

  

Example:

// hydrate previous data into a Mongoose document

const mongooseCandy = Candy.hydrate({ _id: '54108337212ffb6d459f854c', type: 'jelly bean' });

Model.init()

This function is responsible for initializing the underlying connection in MongoDB based on schema options. This function performs the following operations:

  

createCollection() unless autoCreate option is turned off

ensureIndexes() unless autoIndex option is turned off

createSearchIndex() on all schema search indexes if autoSearchIndex is enabled.

Mongoose calls this function automatically when a model is a created using mongoose.model() or connection.model(), so you don't need to call init() to trigger index builds.

  

However, you may need to call init() to get back a promise that will resolve when your indexes are finished. Calling await Model.init() is helpful if you need to wait for indexes to build before continuing. For example, if you want to wait for unique indexes to build before continuing with a test case.

  

Example:

const eventSchema = new Schema({ thing: { type: 'string', unique: true } })

// This calls `Event.init()` implicitly, so you don't need to call

// `Event.init()` on your own.

const Event = mongoose.model('Event', eventSchema);

  

await Event.init();

console.log('Indexes are done building!');

Model.insertMany()

Parameters:

doc(s) «Array|Object|[object Object]»

[options] «Object» see the mongodb driver options

[options.ordered=true] «Boolean» if true, will fail fast on the first error encountered. If false, will insert all the documents it can and report errors later. An insertMany() with ordered = false is called an "unordered" insertMany().

[options.rawResult=false] «Boolean» if false, the returned promise resolves to the documents that passed mongoose document validation. If true, will return the raw result from the MongoDB driver with a mongoose property that contains validationErrors and results if this is an unordered insertMany.

[options.lean=false] «Boolean» if true, skips hydrating the documents. This means Mongoose will not cast, validate, or apply defaults to any of the documents passed to insertMany(). This option is useful if you need the extra performance, but comes with data integrity risk. Consider using with castObject() and applyDefaults().

[options.limit=null] «Number» this limits the number of documents being processed (validation/casting) by mongoose in parallel, this does NOT send the documents in batches to MongoDB. Use this option if you're processing a large number of documents and your app is running out of memory.

[options.populate=null] «String|Object|Array» populates the result documents. This option is a no-op if rawResult is set.

[options.throwOnValidationError=false] «Boolean» If true and ordered: false, throw an error if one of the operations failed validation, but all valid operations completed successfully.

Returns:

«Promise» resolving to the raw result from the MongoDB driver if options.rawResult was true, or the documents that passed validation, otherwise

Shortcut for validating an array of documents and inserting them into MongoDB if they're all valid. This function is faster than .create() because it only sends one operation to the server, rather than one for each document.

  

Mongoose always validates each document before sending insertMany to MongoDB. So if one document has a validation error, no documents will be saved, unless you set the ordered option to false.

  

This function does not trigger save middleware.

  

This function triggers the following middleware.

  

insertMany()

Example:

const docs = await Movies.insertMany([

  { name: 'Star Wars' },

  { name: 'The Empire Strikes Back' }

]);

docs[0].name; // 'Star Wars'

  

// Return raw result from MongoDB

const result = await Movies.insertMany([

  { name: 'Star Wars' },

  { name: 'The Empire Strikes Back' }

], { rawResult: true });

Model.insertOne()

Parameters:

doc «Object|Document» Document to insert, as a POJO or Mongoose document

[options] «Object» Options passed down to save().

Returns:

«Promise<Document>» resolves to the saved document

Shortcut for saving one document to the database. MyModel.insertOne(obj, options) is almost equivalent to new MyModel(obj).save(options). The difference is that insertOne() checks if obj is already a document, and checks for discriminators.

  

This function triggers the following middleware.

  

save()

Example:

// Insert one new `Character` document

const character = await Character.insertOne({ name: 'Jean-Luc Picard' });

character.name; // 'Jean-Luc Picard'

  

// Create a new character within a transaction.

await Character.insertOne({ name: 'Jean-Luc Picard' }, { session });

Model.inspect()

Helper for console.log. Given a model named 'MyModel', returns the string 'Model { MyModel }'.

  

Example:

const MyModel = mongoose.model('Test', Schema({ name: String }));

MyModel.inspect(); // 'Model { Test }'

console.log(MyModel); // Prints 'Model { Test }'

Model.listIndexes()

Returns:

«Promise»

Lists the indexes currently defined in MongoDB. This may or may not be the same as the indexes defined in your schema depending on whether you use the autoIndex option and if you build indexes manually.

  

Model.listSearchIndexes()

Parameters:

[options] «Object»

Returns:

«Promise<Array>»

List all Atlas search indexes on this model's collection. This function only works when connected to MongoDB Atlas.

  

Example:

const schema = new Schema({ name: { type: String, unique: true } });

const Customer = mongoose.model('Customer', schema);

  

await Customer.createSearchIndex({ name: 'test', definition: { mappings: { dynamic: true } } });

const res = await Customer.listSearchIndexes(); // Includes `[{ name: 'test' }]`

Model.namespace()

Return the MongoDB namespace for this model as a string. The namespace is the database name, followed by '.', followed by the collection name.

  

Example:

const conn = mongoose.createConnection('mongodb://127.0.0.1:27017/mydb');

const TestModel = conn.model('Test', mongoose.Schema({ name: String }));

  

TestModel.namespace(); // 'mydb.tests'

Model.populate()

Parameters:

docs «Document|Array» Either a single document or array of documents to populate.

options «Object|String» Either the paths to populate or an object specifying all parameters

[options.path=null] «string» The path to populate.

[options.populate=null] «string|PopulateOptions» Recursively populate paths in the populated documents. See deep populate docs.

[options.retainNullValues=false] «boolean» By default, Mongoose removes null and undefined values from populated arrays. Use this option to make populate() retain null and undefined array entries.

[options.getters=false] «boolean» If true, Mongoose will call any getters defined on the localField. By default, Mongoose gets the raw value of localField. For example, you would need to set this option to true if you wanted to add a lowercase getter to your localField.

[options.clone=false] «boolean» When you do BlogPost.find().populate('author'), blog posts with the same author will share 1 copy of an author doc. Enable this option to make Mongoose clone populated docs before assigning them.

[options.match=null] «Object|Function» Add an additional filter to the populate query. Can be a filter object containing MongoDB query syntax, or a function that returns a filter object.

[options.skipInvalidIds=false] «Boolean» By default, Mongoose throws a cast error if localField and foreignField schemas don't line up. If you enable this option, Mongoose will instead filter out any localField properties that cannot be casted to foreignField's schema type.

[options.perDocumentLimit=null] «Number» For legacy reasons, limit with populate() may give incorrect results because it only executes a single query for every document being populated. If you set perDocumentLimit, Mongoose will ensure correct limit per document by executing a separate query for each document to populate(). For example, .find().populate({ path: 'test', perDocumentLimit: 2 }) will execute 2 additional queries if .find() returns 2 documents.

[options.strictPopulate=true] «Boolean» Set to false to allow populating paths that aren't defined in the given model's schema.

[options.options=null] «Object» Additional options like limit and lean.

[options.transform=null] «Function» Function that Mongoose will call on every populated document that allows you to transform the populated document.

[options.forceRepopulate=true] «Boolean» Set to false to prevent Mongoose from repopulating paths that are already populated

[options.ordered=false] «Boolean» Set to true to execute any populate queries one at a time, as opposed to in parallel. Set this option to true if populating multiple paths or paths with multiple models in transactions.

Returns:

«Promise»

Populates document references.

  

Changed in Mongoose 6: the model you call populate() on should be the "local field" model, not the "foreign field" model.

  

Available top-level options:

path: space delimited path(s) to populate

select: optional fields to select

match: optional query conditions to match

model: optional name of the model to use for population

options: optional query options like sort, limit, etc

justOne: optional boolean, if true Mongoose will always set path to a document, or null if no document was found. If false, Mongoose will always set path to an array, which will be empty if no documents are found. Inferred from schema by default.

strictPopulate: optional boolean, set to false to allow populating paths that aren't in the schema.

forceRepopulate: optional boolean, defaults to true. Set to false to prevent Mongoose from repopulating paths that are already populated

Example:

const Dog = mongoose.model('Dog', new Schema({ name: String, breed: String }));

const Person = mongoose.model('Person', new Schema({

  name: String,

  pet: { type: mongoose.ObjectId, ref: 'Dog' }

}));

  

const pets = await Pet.create([

  { name: 'Daisy', breed: 'Beagle' },

  { name: 'Einstein', breed: 'Catalan Sheepdog' }

]);

  

// populate many plain objects

const users = [

  { name: 'John Wick', dog: pets[0]._id },

  { name: 'Doc Brown', dog: pets[1]._id }

];

await User.populate(users, { path: 'dog', select: 'name' });

users[0].dog.name; // 'Daisy'

users[0].dog.breed; // undefined because of `select`

Model.prototype.$model()

Parameters:

[name] «String» model name

Returns:

«Model»

Returns the model instance used to create this document if no name specified. If name specified, returns the model with the given name.

  

Example:

const doc = new Tank({});

doc.$model() === Tank; // true

await doc.$model('User').findById(id);

Model.prototype.$where

Type:

«property»

Additional properties to attach to the query when calling save() and isNew is false.

  

Model.prototype.base

Type:

«property»

Base Mongoose instance the model uses.

  

Model.prototype.baseModelName

Type:

«property»

If this is a discriminator model, baseModelName is the name of the base model.

  

Model.prototype.collection

Type:

«property»

The collection instance this model uses. A Mongoose collection is a thin wrapper around a [MongoDB Node.js driver collection](MongoDB Node.js driver collection). Using Model.collection means you bypass Mongoose middleware, validation, and casting.

  

This property is read-only. Modifying this property is a no-op.

  

Model.prototype.collection

Type:

«property»

Collection the model uses.

  

Model.prototype.db

Type:

«property»

Connection the model uses.

  

Model.prototype.deleteOne()

Returns:

«Query» Query

Delete this document from the db. Returns a Query instance containing a deleteOne operation by this document's _id.

  

Example:

await product.deleteOne();

await Product.findById(product._id); // null

Since deleteOne() returns a Query, the deleteOne() will not execute unless you use either await, .then(), .catch(), or .exec()

  

Example:

product.deleteOne(); // Doesn't do anything

product.deleteOne().exec(); // Deletes the document, returns a promise

Model.prototype.discriminators

Type:

«property»

Registered discriminators for this model.

  

Model.prototype.increment()

See:

versionKeys

Signal that we desire an increment of this documents version.

  

Example:

const doc = await Model.findById(id);

doc.increment();

await doc.save();

Model.prototype.model()

Parameters:

[name] «String» model name

Returns:

«Model»

Returns the model instance used to create this document if no name specified. If name specified, returns the model with the given name.

  

Example:

const doc = new Tank({});

doc.$model() === Tank; // true

await doc.$model('User').findById(id);

Model.prototype.modelName

Type:

«property»

The name of the model

  

Model.prototype.save()

Parameters:

[options] «Object» options optional options

[options.session=null] «Session» the session associated with this save operation. If not specified, defaults to the document's associated session.

[options.safe] «Object» (DEPRECATED) overrides schema's safe option. Use the w option instead.

[options.validateBeforeSave] «Boolean» set to false to save without validating.

[options.validateModifiedOnly=false] «Boolean» if true, Mongoose will only validate modified paths, as opposed to modified paths and required paths.

[options.w] «Number|String» set the write concern. Overrides the schema-level writeConcern option

[options.j] «Boolean» set to true for MongoDB to wait until this save() has been journaled before resolving the returned promise. Overrides the schema-level writeConcern option

[options.wtimeout] «Number» sets a timeout for the write concern. Overrides the schema-level writeConcern option.

[options.checkKeys=true] «Boolean» the MongoDB driver prevents you from saving keys that start with '$' or contain '.' by default. Set this option to false to skip that check. See restrictions on field names

[options.timestamps=true] «Boolean» if false and timestamps are enabled, skip timestamps for this save().

[options.pathsToSave] «Array» An array of paths that tell mongoose to only validate and save the paths in pathsToSave.

Returns:

«Promise»

See:

middleware

Saves this document by inserting a new document into the database if document.isNew is true, or sends an updateOne operation with just the modified paths if isNew is false.

  

Example:

product.sold = Date.now();

product = await product.save();

If save is successful, the returned promise will fulfill with the document saved.

  

Example:

const newProduct = await product.save();

newProduct === product; // true

Model.recompileSchema()

Returns:

«undefined,void»

Apply changes made to this model's schema after this model was compiled. By default, adding virtuals and other properties to a schema after the model is compiled does nothing. Call this function to apply virtuals and properties that were added later.

  

Example:

const schema = new mongoose.Schema({ field: String });

const TestModel = mongoose.model('Test', schema);

TestModel.schema.virtual('myVirtual').get(function() {

  return this.field + ' from myVirtual';

});

const doc = new TestModel({ field: 'Hello' });

doc.myVirtual; // undefined

  

TestModel.recompileSchema();

doc.myVirtual; // 'Hello from myVirtual'

Model.replaceOne()

Parameters:

filter «Object»

doc «Object»

[options] «Object» optional see Query.prototype.setOptions()

[options.strict] «Boolean|String» overwrites the schema's strict mode option

[options.upsert=false] «Boolean» if true, and no documents found, insert a new document

[options.writeConcern=null] «Object» sets the write concern for replica sets. Overrides the schema-level write concern

[options.timestamps=null] «Boolean» If set to false and schema-level timestamps are enabled, skip timestamps for this update. Does nothing if schema-level timestamps are not set.

[options.translateAliases=null] «Boolean» If set to true, translates any schema-defined aliases in filter, projection, update, and distinct. Throws an error if there are any conflicts where both alias and raw property are defined on the same object.

Returns:

«Query»

See:

Query docs

UpdateResult

Replace the existing document with the given document (no atomic operators like $set).

  

Example:

const res = await Person.replaceOne({ _id: 24601 }, { name: 'Jean Valjean' });

res.matchedCount; // Number of documents matched

res.modifiedCount; // Number of documents modified

res.acknowledged; // Boolean indicating the MongoDB server received the operation.

res.upsertedId; // null or an id containing a document that had to be upserted.

res.upsertedCount; // Number indicating how many documents had to be upserted. Will either be 0 or 1.

This function triggers the following middleware.

  

replaceOne()

Model.schema

Type:

«property»

Schema the model uses.

  

Model.startSession()

Parameters:

[options] «Object» see the mongodb driver options

[options.causalConsistency=true] «Boolean» set to false to disable causal consistency

Returns:

«Promise<ClientSession>» promise that resolves to a MongoDB driver ClientSession

Requires MongoDB >= 3.6.0. Starts a MongoDB session for benefits like causal consistency, retryable writes, and transactions.

  

Calling MyModel.startSession() is equivalent to calling MyModel.db.startSession().

  

This function does not trigger any middleware.

  

Example:

const session = await Person.startSession();

let doc = await Person.findOne({ name: 'Ned Stark' }, null, { session });

await doc.deleteOne();

// `doc` will always be null, even if reading from a replica set

// secondary. Without causal consistency, it is possible to

// get a doc back from the below query if the query reads from a

// secondary that is experiencing replication lag.

doc = await Person.findOne({ name: 'Ned Stark' }, null, { session, readPreference: 'secondary' });

Model.syncIndexes()

Parameters:

[options] «Object» options to pass to ensureIndexes()

[options.background=null] «Boolean» if specified, overrides each index's background property

[options.hideIndexes=false] «Boolean» set to true to hide indexes instead of dropping. Requires MongoDB server 4.4 or higher

Returns:

«Promise»

Makes the indexes in MongoDB match the indexes defined in this model's schema. This function will drop any indexes that are not defined in the model's schema except the _id index, and build any indexes that are in your schema but not in MongoDB.

  

See the introductory blog post for more information.

  

Example:

const schema = new Schema({ name: { type: String, unique: true } });

const Customer = mongoose.model('Customer', schema);

await Customer.collection.createIndex({ age: 1 }); // Index is not in schema

// Will drop the 'age' index and create an index on `name`

await Customer.syncIndexes();

You should be careful about running syncIndexes() on production applications under heavy load, because index builds are expensive operations, and unexpected index drops can lead to degraded performance. Before running syncIndexes(), you can use the diffIndexes() function to check what indexes syncIndexes() will drop and create.

  

Example:

const { toDrop, toCreate } = await Model.diffIndexes();

toDrop; // Array of strings containing names of indexes that `syncIndexes()` will drop

toCreate; // Array of strings containing names of indexes that `syncIndexes()` will create

Model.translateAliases()

Parameters:

fields «Object» fields/conditions that may contain aliased keys

[errorOnDuplicates] «Boolean» if true, throw an error if there's both a key and an alias for that key in fields

Returns:

«Object» the translated 'pure' fields/conditions

Translate any aliases fields/conditions so the final query or document object is pure

  

Example:

await Character.find(Character.translateAliases({

   '名': 'Eddard Stark' // Alias for 'name'

});

By default, translateAliases() overwrites raw fields with aliased fields. So if n is an alias for name, { n: 'alias', name: 'raw' } will resolve to { name: 'alias' }. However, you can set the errorOnDuplicates option to throw an error if there are potentially conflicting paths. The translateAliases option for queries uses errorOnDuplicates.

  

Note:

Only translate arguments of object type anything else is returned raw

  

Model.updateMany()

Parameters:

filter «Object»

update. «Object|Array» If array, this update will be treated as an update pipeline and not casted.

[options] «Object» optional see Query.prototype.setOptions()

[options.strict] «Boolean|String» overwrites the schema's strict mode option

[options.upsert=false] «Boolean» if true, and no documents found, insert a new document

[options.writeConcern=null] «Object» sets the write concern for replica sets. Overrides the schema-level write concern

[options.timestamps=null] «Boolean» If set to false and schema-level timestamps are enabled, skip timestamps for this update. Does nothing if schema-level timestamps are not set.

[options.translateAliases=null] «Boolean» If set to true, translates any schema-defined aliases in filter, projection, update, and distinct. Throws an error if there are any conflicts where both alias and raw property are defined on the same object.

[options.overwriteDiscriminatorKey=false] «Boolean» Mongoose removes discriminator key updates from update by default, set overwriteDiscriminatorKey to true to allow updating the discriminator key

Returns:

«Query»

See:

Query docs

MongoDB docs

UpdateResult

Same as updateOne(), except MongoDB will update all documents that match filter (as opposed to just the first one) regardless of the value of the multi option.

  

Note updateMany will not fire update middleware. Use pre('updateMany') and post('updateMany') instead.

  

Example:

const res = await Person.updateMany({ name: /Stark$/ }, { isDeleted: true });

res.matchedCount; // Number of documents matched

res.modifiedCount; // Number of documents modified

res.acknowledged; // Boolean indicating the MongoDB server received the operation. This may be false if Mongoose did not send an update to the server because the update was empty.

res.upsertedId; // null or an id containing a document that had to be upserted.

res.upsertedCount; // Number indicating how many documents had to be upserted. Will either be 0 or 1.

  

// Other supported syntaxes

await Person.find({ name: /Stark$/ }).updateMany({ isDeleted: true }); // Using chaining syntax

await Person.find().updateMany({ isDeleted: true }); // Set `isDeleted` on _all_ Person documents

This function triggers the following middleware.

  

updateMany()

Model.updateOne()

Parameters:

filter «Object»

update. «Object|Array» If array, this update will be treated as an update pipeline and not casted.

[options] «Object» optional see Query.prototype.setOptions()

[options.strict] «Boolean|String» overwrites the schema's strict mode option

[options.upsert=false] «Boolean» if true, and no documents found, insert a new document

[options.writeConcern=null] «Object» sets the write concern for replica sets. Overrides the schema-level write concern

[options.timestamps=null] «Boolean» If set to false and schema-level timestamps are enabled, skip timestamps for this update. Note that this allows you to overwrite timestamps. Does nothing if schema-level timestamps are not set.

[options.translateAliases=null] «Boolean» If set to true, translates any schema-defined aliases in filter, projection, update, and distinct. Throws an error if there are any conflicts where both alias and raw property are defined on the same object.

[options.overwriteDiscriminatorKey=false] «Boolean» Mongoose removes discriminator key updates from update by default, set overwriteDiscriminatorKey to true to allow updating the discriminator key

Returns:

«Query»

See:

Query docs

MongoDB docs

UpdateResult

Update only the first document that matches filter.

  

Use replaceOne() if you want to overwrite an entire document rather than using atomic operators like $set.

Example:

const res = await Person.updateOne({ name: 'Jean-Luc Picard' }, { ship: 'USS Enterprise' });

res.matchedCount; // Number of documents matched

res.modifiedCount; // Number of documents modified

res.acknowledged; // Boolean indicating the MongoDB server received the operation. This may be false if Mongoose did not send an update to the server because the update was empty.

res.upsertedId; // null or an id containing a document that had to be upserted.

res.upsertedCount; // Number indicating how many documents had to be upserted. Will either be 0 or 1.

  

// Other supported syntaxes

await Person.findOne({ name: 'Jean-Luc Picard' }).updateOne({ ship: 'USS Enterprise' }); // Using chaining syntax

await Person.updateOne({ ship: 'USS Enterprise' }); // Updates first doc's `ship` property

This function triggers the following middleware.

  

updateOne()

Model.updateSearchIndex()

Parameters:

name «String»

definition «Object»

Returns:

«Promise»

Update an existing Atlas search index. This function only works when connected to MongoDB Atlas.

  

Example:

const schema = new Schema({ name: { type: String, unique: true } });

const Customer = mongoose.model('Customer', schema);

await Customer.updateSearchIndex('test', { mappings: { dynamic: true } });

Model.useConnection()

Parameters:

[Connection] «» connection The new connection to use

Returns:

«» [Model] this

Changes the Connection instance this model uses to make requests to MongoDB. This function is most useful for changing the Connection that a Model defined using mongoose.model() uses after initialization.

  

Example:

await mongoose.connect('mongodb://127.0.0.1:27017/db1');

const UserModel = mongoose.model('User', mongoose.Schema({ name: String }));

UserModel.connection === mongoose.connection; // true

  

const conn2 = await mongoose.createConnection('mongodb://127.0.0.1:27017/db2').asPromise();

UserModel.useConnection(conn2); // `UserModel` now stores documents in `db2`, not `db1`

  

UserModel.connection === mongoose.connection; // false

UserModel.connection === conn2; // true

  

conn2.model('User') === UserModel; // true

mongoose.model('User'); // Throws 'MissingSchemaError'

Note: useConnection() does not apply any connection-level plugins from the new connection. If you use useConnection() to switch a model's connection, the model will still have the old connection's plugins.

  

Model.validate()

Parameters:

obj «Object»

pathsOrOptions «Object|Array|String»

[context] «Object»

Returns:

«Promise<Object>» casted and validated copy of obj if validation succeeded

Casts and validates the given object against this model's schema, passing the given context to custom validators.

  

Example:

const Model = mongoose.model('Test', Schema({

  name: { type: String, required: true },

  age: { type: Number, required: true }

});

  

try {

  await Model.validate({ name: null }, ['name'])

} catch (err) {

  err instanceof mongoose.Error.ValidationError; // true

  Object.keys(err.errors); // ['name']

}

Model.watch()

Parameters:

[pipeline] «Array»

[options] «Object» see the mongodb driver options

[options.hydrate=false] «Boolean» if true and fullDocument: 'updateLookup' is set, Mongoose will automatically hydrate fullDocument into a fully fledged Mongoose document

Returns:

«ChangeStream» mongoose-specific change stream wrapper, inherits from EventEmitter

Requires a replica set running MongoDB >= 3.6.0. Watches the underlying collection for changes using MongoDB change streams.

  

This function does not trigger any middleware. In particular, it does not trigger aggregate middleware.

  

The ChangeStream object is an event emitter that emits the following events:

  

'change': A change occurred, see below example

'error': An unrecoverable error occurred. In particular, change streams currently error out if they lose connection to the replica set primary. Follow this GitHub issue for updates.

'end': Emitted if the underlying stream is closed

'close': Emitted if the underlying stream is closed

Example:

const doc = await Person.create({ name: 'Ned Stark' });

const changeStream = Person.watch().on('change', change => console.log(change));

// Will print from the above `console.log()`:

// { _id: { _data: ... },

//   operationType: 'delete',

//   ns: { db: 'mydb', coll: 'Person' },

//   documentKey: { _id: 5a51b125c5500f5aa094c7bd } }

await doc.deleteOne();

Model.where()

Parameters:

path «String»

[val] «Object» optional value

Returns:

«Query»

Creates a Query, applies the passed conditions, and returns the Query.

  

For example, instead of writing:

  

User.find({ age: { $gte: 21, $lte: 65 } });

we can instead write:

  

User.where('age').gte(21).lte(65).exec();

Since the Query class also supports where you can continue chaining

  

User

.where('age').gte(21).lte(65)

.where('name', /^b/i)

... etc

  

Localize

Full Stack Engineer

Anywhere

View more jobs!

  

  

how revelant are these to wah we are doing?