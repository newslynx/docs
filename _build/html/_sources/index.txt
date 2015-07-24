.. particle documentation master file, created by
   sphinx-quickstart on Wed Dec 25 21:19:20 2013.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

.. image::  ./_static/merlynne-ears.png
   :align:   left
   :width:   125px

**NewsLynx**: own your analytics
================================

NewsLynx is a research project from the Tow Center for Digital Journalism at Columbia University by Brian Abelson & Michael Keller. 

Read our `white paper <http://towcenter.org/wp-content/uploads/2015/06/Tow_Center_NewsLynx_Full_Report.pdf>`_ (PDF, released June 2015) or the `Executive Summary <http://towcenter.org/research/the-newslynx-impact-tracker-produced-these-key-ideas/>`_ if you're in a hurry. If you want more, you can read our project update posts that look at `the problem of scraping the whole internet <http://towcenter.org/hyper-compensation-ted-nelson-and-the-impact-of-journalism/>`_, why `taxonomies are like Jorge Luis Borges <http://towcenter.org/tow-fellows-brian-abelson-and-michael-keller-to-study-the-impact-of-journalism/>`_ or read `a walkthrough of our initial prototype <http://towcenter.org/the-newslynx-tool-for-impact-tracking-a-walkthrough/>`_. 

All of our code is open source and `on GitHub <http://github.com/newslynx>`_. Please read license information before using or `contact us <mailto:contact@newslynx.org>`_ if you have questions. Happy Lynxing!

.. raw:: html

   <div style="clear:both;"></div>

   <style>
     #tow-logo{
       position: absolute;
       top: 15px;
       right: 15px;
     }
     #tow-logo img{
       width: 75px;
       height: 75px;
     }
    @media all and (max-width: 768px){
      #tow-logo{
        background-color: #fff;
        border-radius: 3px;
        top: 3px;
        right: 4px;
        height: 57px;
        padding: 3px;
      }
      #tow-logo img{
        width: 50px !important;
        height: 50px !important;
      }
    }
    @media all and (max-width: 1211px) and (min-width: 768px){
      #tow-logo {
        display: none
      }
    }
   </style>
   <a id="tow-logo" href="http://towcenter.org/">
    <img src="_static/towcenter.png"/>
   </a>

Contents
--------

.. toctree::
   :maxdepth: 3

   install
   config
   getting-started
   sous-chefs 
   metrics
   recipes
   taxonomy 
   content-items
   events
   api
   app
   cli
   writing-sous-chefs
   

