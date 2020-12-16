# Authorship

Stable tag: 0.1.0  
Requires at least: 5.4  
Tested up to: 5.6  
Requires PHP: 7.2  
License: GPL v3 or later  
Contributors: johnbillion, humanmade  

A modern approach to author attribution in WordPress.

## Description

Authorship is a modern approach to author attribution in WordPress. It supports multiple authors and guest authors, provides a great UI, and treats API access to author data as a first-class citizen.

Authorship is currently geared toward developers who are implementing custom solutions on WordPress. For example, it doesn't provide an option to automatically display author profiles at the bottom of a post. In the future it will include wider support for existing themes and useful features for implementors and site builders.

---

 * [Features](#features)
 * [Design decisions](#design-decisions)
 * [Team](#team)
 * [License](#license)
 * [Alternatives](#alternatives)

---

## Features

* [X] Multiple authors per post
* [X] Guest authors (that can be created in place on the post editing screen)
* [X] A convenient and user-friendly UI that feels like a part of WordPress
* [X] Works with the block editor
* [ ] Works with the classic editor
* [X] Full CRUD support in the REST API and WP-CLI
* [ ] Full support in RSS and Atom feeds
* [ ] Fine-grained user permission controls
* [ ] Plenty of filters and actions

_Features without a checkmark are still work in progress._

## Installation

* Clone this repo into your plugins directory
* Install the dependencies:  
  `composer install && npm install`
* Start the dev server:  
  `npm run start`

## Design decisions

Why another multi-author plugin? What about Co-Authors Plus or Bylines or PublishPress Authors?

Firstly, those plugins are great and have served us well over the years, however they all suffer from similar problems:

* API: Lack of support for exposing author data beyond the theme, for example via the REST API, WP-CLI, and feeds
* UI: Limited or custom UI that doesn't feel like a part of WordPress
* Users: An unnecessary distinction between guest authors and actual WordPress users

Let's look at these points in detail and explain how Authorship addresses them:

### API design decisions

There's a lot more to a WordPress site than just its theme. Data can be consumed via its APIs and feeds, so these need to be treated as first-class citizens when exposing the attributed authors of posts.

Authorship provides:

* Read and write access to attributed authors via an `authorship` field on the default `wp/v2/posts` REST API endpoints
* Ability to create guest authors via the `authorship/v1/users` REST API endpoint
* Read-only access to users who can be attributed to a post via the `authorship/v1/users` REST API endpoint
* Ability to specify attributed authors in WP-CLI with the `--authorship` flag

On the roadmap:

* Full support for RSS and Atom feed output
* Support for XML-RPC
* Support for WPGraphQL

### UI design decisions

We'd love it if you activated Authorship and then forgot that its features are provided by a plugin. The UI provides convenient functionality without looking out of place, both in the block editor and the classic editor.

### User design decisions

Existing plugins that provide guest author functionality make a distinction between a guest author and a real WordPress user. A guest author exists only as a taxonomy term, which complicates the UX and creates inconsistencies and duplication in the data.

Authorship creates a real WordPress user account for each guest author, which provides several advantages:

* No custom administration screens for managing guest authors separately from regular users
* Plugins that customise user profiles work for guest authors too
* Consistent data structure - you only ever deal with `WP_User` objects
* No need to keep data in sync between a user and their "author" profile
* Promoting a guest author to a functional user is done just by changing their user role

## REST API

The following REST API endpoints and fields are available:

### authorship/v1/users endpoint

This endpoint allows:

* Searching all users who can be attributed to content
* Creating guest authors

### authorship field

This field is added to the endpoint for all post types that have post type support for `author`, for example `wp/v2/posts`. This field is readable and writable and accepts and provides an array of user IDs attributed to the post.

## WP-CLI

The following WP-CLI flags are available:

### --authorship flag

When creating or updating posts, the `--authorship` flag can be used to specify the attributed users of the post. The flag accepts a comma-separated list of user IDs. Examples:

* `wp post create --post_title="My New Post" --authorship=4,11`
* `wp post update 220 --authorship=13`

When this flag is *not* set:

* When creating a new post, if the `--post_author` flag is set then it will be used for authorship
* When updating an existing post, no change will be made to authorship

## Team

Authorship is developed and maintained by [Human Made](https://humanmade.com) and [Altis](https://www.altis-dxp.com). Its initial development was funded by [Siemens](https://www.siemens.com).

[<img src="assets/images/hm-logo.png" width="207" height="86" alt="">](https://humanmade.com)
[<img src="assets/images/altis-logo.png" width="207" height="86" alt="">](https://www.altis-dxp.com)
[<img src="assets/images/siemens-logo.png" width="215" height="86" alt="">](https://www.siemens.com)

## License

This program is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation; either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

## Alternatives

If the Authorship plugin doesn't suit your needs, try these alternatives:

* [PublishPress Authors](https://wordpress.org/plugins/publishpress-authors/)
* [Co-Authors Plus](https://wordpress.org/plugins/co-authors-plus/)
* [Guest Author](https://wordpress.org/plugins/guest-author/)
