### Part 0
I feel that with my project I would like to follow through with an idea in which I created an app
that can simulate a golf round for the user. While many apps keep track of courses, your handicap, and
other different pats of your game, very few actually show you which holes you struggle with the most,
which is what my app intends to do. I would most likely use a data base of golf courses, if one exists,
and it would be a phone app. The end goal is hopefully to create an app that will help you hone your skills even more. 

![latex](latex.PNG)

### Part 1
Venue:
11 contributors

49398 lines of code in title

First commit was Tuesday December 15, 2015

Last commit was December 11, 2018

Current Branches are: 

origin/HEAD -> origin/dev

origin/ListBranch

origin/better_form_validation

origin/cas_redirect

origin/database-logging

origin/delete

origin/dev

origin/flow-types

origin/frontend-fixes

origin/great-venue

origin/kousuke

origin/list

origin/manual_validation

origin/master

origin/new_dev

origin/pending_icon

origin/return-promise

origin/return-statements

origin/revert-479-database-logging

origin/roles

origin/rpi

origin/security-fixes

origin/sign-up-bug

gitstat file [gitstat_venie](gitstat_venue.PNG)

Inside the file are the different grouce video.

Tornado gource [link to tornado](https://www.youtube.com/watch?v=n5TCfNHC6Rw&feature=youtu.be)

### Part 2

Below are the changes to markdown.py and test_markdown_unittest.py

#markdown.py

"""
 Markdown.py
 0. just print whatever is passed in to stdin
 0. if filename passed in as a command line parameter, 
    then print file instead of stdin
 1. wrap input in paragraph tags
 2. convert single asterisk or underscore pairs to em tags
 3. convert double asterisk or underscore pairs to strong tags

"""

import fileinput
import re

def convertStrong(line):
  line = str(re.sub(r'\*\*(.*)\*\*', r'<strong>\1</strong>', line))
  line = str(re.sub(r'__(.*)__', r'<strong>\1</strong>', line))
  return line

def convertEm(line):
  line = str(re.sub(r'\*(.*)\*', r'<em>\1</em>', line))
  line = str(re.sub(r'_(.*)_', r'<em>\1</em>', line))
  return line

def convertH1(line):
  line = str(re.sub(r'\#(.*)',r'<h1>\1</h1>', line))
  line = str(re.sub(r'_(.*)_', r'<h1>\1</h1>', line))
  return line

def convertH2(line):
  line = str(re.sub(r'\#\#(.*)',r'<h2>\1</h2>',line))
  line = str(re.sub(r'_(.*)_', r'<h2>\1</h2>', line))
  return line

def convertH3(line):
  line = str(re.sub(r'\#\#\#(.*)',r'<h3>\1</h3>',line))
  line = str(re.sub(r'_(.*)_', r'<h3>\1</h3>', line))
  return line

def convertBlockFirst(line, start):
  if start == False and line[0] == '>':
    start = True
    print('<blockquote>', end = '')
    line = line.strip('>')
  return line, start

def convertBlockMid(line, start):
  if (start == True and line[0] == '>'):
    line = line.strip('>')
  return line

start = False;

for line in fileinput.input():
  line = line.rstrip()
  if (start == True and line[0] != '>'):
    start = False;
    print('</blockquote>', end = '')
  line = convertBlockMid(line, start)
  line, start = convertBlockFirst(line, start)
  line = convertStrong(line)
  line = convertEm(line)
  line = convertH3(line)
  line = convertH2(line)
  line = convertH1(line)
  print('<p>' + line + '</p>', end='')
  
  #test_markdown_unittest.py
'''
Test markdown.py with unittest
To run tests:
    python test_markdown_unittest.py
'''

import unittest
from markdown_adapter import run_markdown

class TestMarkdownPy(unittest.TestCase):

    def setUp(self):
        pass

    def test_non_marked_lines(self):
        '''
        Non-marked lines should only get 'p' tags around all input
        '''
        self.assertEqual( 
                run_markdown('this line has no special handling'), 
                '<p>this line has no special handling</p>')

    def test_em(self):
        '''
        Lines surrounded by asterisks should be wrapped in 'em' tags
        '''
        self.assertEqual( 
                run_markdown('*this should be wrapped in em tags*'),
                '<p><em>this should be wrapped in em tags</em></p>')

    def test_strong(self):
        '''
        Lines surrounded by double asterisks should be wrapped in 'strong' tags
        '''
        self.assertEqual( 
                run_markdown('**this should be wrapped in strong tags**'),
                '<p><strong>this should be wrapped in strong tags</strong></p>')

    def test_h(self):
        '''
        Lines with # at front wrapped in <h1> tags
        Lines with ## at front wrapped in <h2> tags
        Lines with ### at front wrapped in <h3> tags
        '''
        self.assertEqual(
                run_markdown('#this should be wrapped in h1 tags'),
                '<p><h1>this should be wrapped in h1 tags</h1></p>')

        self.assertEqual(
                run_markdown('##this should be wrapped in h2 tags'),
                '<p><h2>this should be wrapped in h2 tags</h2></p>')

        self.assertEqual(
                run_markdown('###this should be wrapped in h3 tags'),
                '<p><h3>this should be wrapped in h3 tags</h3></p>')

    def test_block(self):
        self.assertEqual(
            run_markdown('>hello there\n>my name is\n>Lucas Standaert\nno more blockquote'),
            '<blockquote><p>hello there</p><p>my name is</p><p>Lucas Standaert</p></blockquote><p>no more blockquote</p>')

if __name__ == '__main__':
    unittest.main()

