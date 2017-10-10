# Thesis - Automation of router configuration

Description text here

* Automation of router configuration
* Name: Joram Puumala
* E-Mail: a1500947@myy.haaga-helia.fi
* Supervisor: Tero Karvinen
* Haaga-Helia University of Applied Sciences
* Keywords: pytnon, vpn, configuration, router, automation

- - -

# Table of Contents

* 1 Introduction
    * 1.1 Background
    * 1.2 About this thesis
    * 1.3 Goals of the study
* 2 Theory/Technologies
	* 2.1 Web Scraping
		* 2.1.0 Web Scraping techniques
		* 2.1.1 Web Scraping tools
	* 2.2 Data manipulation with Regular Expressions
	* 2.3 Data conversion
* 3 Current state
* 4 Automating the process
	* 4.1 Automatic configuration
	* 4.2 Data extraction
	* 4.3 Data conversion
	* 4.4 Integration
* 5 Results
* 6 Future development
* 7 References
* 8 Appendix

# Introduction

## Background

Router orders are getting bigger, new customers and services are stirring the soup. NDC Networks is facing a serious problem, they are running out of resources. Router configuration by hand is no longer an option.

NDC Networks is a small Finnish company based in Espoo. The company is known for its expertise in networks, Virtual Private Network (VPN) management and router configuration. Routers that technicians at NDC Networks configure are mostly mobile routers. Different mobile routers do exist, but the basic idea is that it can be connected to mobile network using a traditional Subscriber Identity Module (SIM) card and is capable of changing its point of attachment to the Internet, moving from one link to another link (Ernst & Lach, 5). Because of the possible different features and demands though, configurations must be unique for each customer.

## About this thesis

## Goals of the study

Main goal of this study is to find out how to decrease used time and configuration errors in router configuration.

Research questions are:

- What is the best technology/technique to configure a router in this case and why?
- How much time can be saved, automation vs hand?
- How many fewer errors/misconfigurations will occur?

- - -

# Theory/Technologies (change title)

## Configuration technologies

## Web Scraping

Extracting patterned data from web pages in the Internet is called Web scraping. There are different uses for Web scraping, one major use is for businesses to track pricing activities of their competitors. Using Web scraping techniques time and labor can be saved in massive amounts. Web scraping can prove itself useful in the realm of research as well (Haddaway, 1-2).

The basic idea behind a Web scraping framework is that it establishes communication with user defined Web page using the HTTP protocol, which is a stateless text-based Internet protocol designated to coordinate the request-response transactions between a client and a Web server, in which the client is typically a Web browser. The "User-Agent" header also plays a big role, because that tells the server whether it is accessed by a robot or a browser. Once the Web scraping framework has retrieved the HTML documents using GET method, contents of interest can be extracted. Because extracting the contents of interest is relevant, regular expressions alone or with a combination of additional logic prove to be powerful and thus are widely adapted. Alternative methods include selector-based languages such as XPath and the CSS selector syntax (Glez-Peña, ?).

Web scraping has some downsides too. It is slower than a simple HTTP request to a web server, because browser waits until the whole Web page is loaded before it allows you to access its elements. What is more, the browser generates more network traffic, which is because of the supplementary files being loaded such as image files, yet they usually don't prove to be useful when Web scraping. One of the biggest issues is that Web pages do change. This may break your code and you have to fix it to make it work again.

### Web Scraping tools

## Data manipulation with Regular Expressions

## Data conversion

- - -

# Current state

- - -

# Automating the process

## Automatic configuration

## Data extraction

## Data Conversion

## Integration

- - -

# Results

- - -

# Future development

- - -

# References

- - -

# Appendix
