---
layout: post
title:  "Unicorn workers under the hood"
date:   2015-06-30 22:43:00
categories: jekyll update
---

You --> URL --> DNS ( avant.com -> IP Address )

DNS --> (Chome -> OS -> Comcast/RCN -> Canonical Lookups (ICANN))
--> IP Address
--> Server on heroku
--> App Instances
--> Front-End Web Server -> Unicorn
--> Unicorn Master --> Queue --> Unicorn Workers
--> Unicorn Worker --> Rails app

whois avant.com

Rack : ruby web server interface

HTTP RESPONSE CODE, HEADERS, BODY
#it takes an env and returns 3 things: response code, header, body
app = Proc.new do |env|
['200', {'Content-Type' => text/html}, ['a barebones rack app']]

rake middleware
use HireFire::Middleware
use Honeybadger::Rack::UserInformer
use Honeybadger::Rack::UserFeedback
use Honeybadger::Rack::ErrorNotifier
use ActionDispatch::Static
use Avant::ReverseProxy
use Rack::Lock
use #<ActiveSupport::Cache::Strategy::LocalCache::Middleware:0x007fe8eae20240>
use Rack::Runtime
use Rack::MethodOverride
use ActionDispatch::RequestId
use RequestStore::Middleware
use Rails::Rack::Logger
use ActionDispatch::ShowExceptions
use ActionDispatch::DebugExceptions
use BetterErrors::Middleware
use ActionDispatch::RemoteIp
use ActionDispatch::Reloader
use ActionDispatch::Callbacks
use ActiveRecord::ConnectionAdapters::ConnectionManagement
use ActiveRecord::QueryCache
use ActionDispatch::Cookies
use ActionDispatch::Session::CookieStore
use ActionDispatch::Flash
use ActionDispatch::ParamsParser
use ActionDispatch::Head
use Rack::ConditionalGet
use Rack::ETag
use ActionDispatch::BestStandardsSupport
use Warden::Manager
use PDFKit::Middleware
use Rack::ContentLength
use MetaRequest::Middlewares::MetaRequestHandler
use MetaRequest::Middlewares::Headers
use MetaRequest::Middlewares::AppRequestHandler
run Avant::Application.routes


HTTP protocol is about document/file location
Resource location and modification

GET 		(REQUEST AN EXISTING RESOURCE)
POST 		(CREATING A NEW RESOURCE)
PUT 		(FULL UPDATE)
PATCH 	(PARTIAL UPDATE)
DELETE 	(DELETE A RESOURCE)