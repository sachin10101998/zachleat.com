---
title: 'Faster YUI DataTable with 5 Lines of Code'
author: Zach Leatherman
layout: post
permalink: /faster-yui-datatable-with-5-lines-of-code/
Warning, written for:
  - YUI 2.4.1
btc_comment_counts:
  - 'a:0:{}'
btc_comment_summary:
  - 'a:0:{}'
bttc_cache:
  - 1299718826:0
categories:
  - JavaScript
tags:
  - DataTable
  - Grid
  - Performance
  - YUI
---

Holy Reflows Batman! The typical usage of a DataTable in the Yahoo User Interface JavaScript library involves passing a string into the constructor signifying the ID attribute of the container you want to attach the DataTable to. However, the YUI DataTable loves the DOM and creating nodes individually using DOM methods. Normally that’d be fine, but one of the first things it does in the constructor is create the table element and attach it to the live DOM. This is a no-no. Now, every time they append a new node (for a new row or a new cell inside of a row), it causes a reflow in the browser! What does this mean? Really bad lag when you insert 40 or 50 rows. Recognize this piece of code?

    var myDataTable = new YAHOO.widget.DataTable&#40;"myContainer", myColumnDefs, myDataSource&#41;;

Straight from the docs. No no no!

Instead, you should pass in an unattached DOM node instead of a string!

Try this code on for size:

    var myDataTable = new YAHOO.widget.DataTable&#40;document.createElement&#40;'div'&#41;, myColumnDefs, myDataSource&#41;; 
    myDataTable.subscribe&#40;'initEvent',function&#40;&#41; &#123;
        var d = document.getElementById&#40;'myContainer'&#41;; // CHANGE THIS -- match the id of the container you want.
        while&#40;d.firstChild&#41; &#123; d.removeChild&#40;d.firstChild&#41;; &#125;; // remove previous DataTables
        d.appendChild&#40;this._elContainer&#41;; &#125;&#41;;
    &#125;