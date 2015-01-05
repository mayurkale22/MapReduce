<h1>learnMapReduce</h>

Just playing with mapReduce

MapReduce is a programming model for data processing. Most important, MapReduce programs are inherently parallel, thus putting very large-scale data analysis into the hands of anyone with enough machines at their disposal.

<h4>Map and Reduce</h4>
MapReduce works by breaking the processing into two phases: the map phase and the reduce phase. Each phase has key-value pairs as input and output, the types of which may be chosen by the programmer. The programmer also specifies two functions: the map function and the reduce function.

<h4>A test run</h4>
<i>% export HADOOP_CLASSPATH=hadoop-examples.jar<br/>
% hadoop MaxTemperature input/ncdc/sample.txt output<br/></i>


<h2>1) PageRank Problem</h2>

PageRank is an algorithm used by the Google web search engine to rank websites in their search engine results. It is named after Larry Page, although many people think it means "rank of pages". 

You can see a brief introduction at: http://en.wikipedia.org/wiki/PageRank. This is a long page to read, but you don't need to understand every detail here. The important part is the algorithm: http://en.wikipedia.org/wiki/PageRank#Algorithm. This is all you need to know about PageRank for this homework assignment. You can skip the matrix and algebraic parts. Focus on how to get PR(A) from PR(B), PR(C) and PR(D). 

Basically, PageRank is trying to do this: Distribute the page's own PR value to all of the linked pages iteratively, and finally get a stable state, which presents the theoretical PR values of all pages. As described on the wiki, you can transfer PR/outlinks of a page to all linked pages, and you can also add a damping factor. Let's ignore the damping factor, focus on the formula:
PR(A) = PR(B)/2 + PR(C) + PR(D)/3

Your task is to implement a simplified PageRank with MapReduce.

To simplify your work, you can assume that we have the following input file:

  A C F 0.166667<br/>
  B D E F 0.166667<br/>
  C A B 0.166667<br/>
  D A B C E F 0.166667<br/>
  E F 0.166667<br/>
  F B C 0.166667<br/>

  The first line, for example, is interpreted as follows:
  “A” means "Page A“.
  "C F" means "Page A" has outlinks to "Page C" and "Page F“.
  "0.166667" is the initial PR value of Page A. 
  
Remember, this is a DIRECTIONAL graph, i.e. links have direction. For instance,  "A C F" means A has outlinks to C and F, and "B D E F" means B has outlinks to D, E and F.

After you read in this input, your MR jobs should parse and process the data, and output the PR value for ONE iteration. This means you only need to use the formula on the data ONCE.

Your output file should look like this, where PR is the pagerank value computed by your program:

A C F PR<br/>
B D E F PR <br/>
C A B PR <br/>
D A B C E F PR <br/>
E F PR <br/>
F B C PR <br/>
