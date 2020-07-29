#### Michał Stawikowski 291138
# Kohonen Report

In this report I will summarize all the experiments that I conducted on the Kohonen network that I implemented. The document is divided into paragraphs in the order of the laboratories.

## 1. Basic tests and  implementation
  
In this part, we have implemented a simple Kohonen network model. Using the **gaussian**  neighborhood function and its **second derivative**, we will try to perform clusterisation on two data sets using different neighborhood sizes. I will try to viusalize outputs in many ways.
  
Before each result I will present the architecture and parameters of the network used.

## Hexagon dataset

#### First try

  
Number of neurons corresponding to the number of clusters.

    net = Kohonen_Network(3,2,2)
    net.train(x, epochs = 10, n_width = 1)

![enter image description here](https://i.ibb.co/hcwSrgW/1.png)

  
Clusters have been well identified, now let's see what happens for more neurons.

#### Gaussian function with different neighborhood size

    net = Kohonen_Network(10,10,2)
    net.train(x, epochs = 10, n_width = size)

![enter image description here](https://i.ibb.co/2czG4YD/2.png)
![enter image description here](https://i.ibb.co/yN2JHdL/3.png)

![enter image description here](https://i.ibb.co/DW39HJ7/4.png)
  
  ![enter image description here](https://i.ibb.co/2M8tvKk/5.png)

As can be seen from the above charts, clusters have more or less been identified, their number corresponds to the number of classes, and their location is similiar to these from the original data. With the growing size of the neighborhood, the uneven density of points in individual clusters increased. Network learning time is quite long compared to MLP. Neurons close at MxN map have simmiliar weights as we can see from indexing.

### Weights Heatmap

Now I will present the visualization, which consists in casting the weights of all neurons on specific colors in the RGB format using two channels for 2d data and three for 3d data.

    net = Kohonen_Network(10,10,2)
    net.train(x, epochs = 3, n_width = 0.4)

 
![enter image description here](https://i.ibb.co/gRnLs11/6.png)

Another way to visualize outputs is heatmap of weights. We can clarly see the division into all clusters here and that close neurons have simlilliar weights.

### Gaussian second derivative

In the meantime, I changed the implementation of the second derivative of the Gaussian function, which, according to my experiments, did much better, so that the parameter neighborhood size in her case was not from 0.1 to 1.

![enter image description here](https://scontent-frx5-1.xx.fbcdn.net/v/t1.15752-9/94707564_2595781907192164_6595323291219525632_n.png?_nc_cat=100&_nc_sid=b96e70&_nc_ohc=HOpC-8e_KLEAX-R8zfb&_nc_ht=scontent-frx5-1.xx&oh=870b2a17ee3262555d430944d04e08e1&oe=5ED4EB1F)

Where sigma is our   blanking function.

    net = Kohonen_Network(10,10,2)
    net.train(x, epochs = 8, n_width = 5, n_fun = "second")

![enter image description here](https://i.ibb.co/wJcFDS6/7.png) 
  
Individual clusters are separated from each other by high output values, which is produced by the second derivative of the Gaussian function. As we can see there are less clusters which represents data groups and more that only separate them.

## Cube dataset
  
The next dataset is 3d points spread around cube.

### First try

Number of neurons corresponding to the number of clusters.

    net = Kohonen_Network(4,2,3)
    net.train(x, epochs = 20, n_width = 0.1)

![enter image description here](https://i.ibb.co/S76rbDd/8.png)

### Gaussian function with different neighborhood size

 

 

    net = Kohonen_Network(7,7,3)
    net.train(x, epochs = 10, n_width = size)

![enter image description here](https://i.ibb.co/9NzRPJ8/9.png)

![enter image description here](https://i.ibb.co/YDVsRHc/10.png)

![enter image description here](https://i.ibb.co/H2d1NGs/11.png)
  
  ![enter image description here](https://i.ibb.co/SsbbHqz/12.png)

In three dimensions it is harder to see, but the network has divided the points in a similar way as they appear in the original set. The higher the size of the neighborhood, the more widespread points are.

### Weights Heatmap

#### Gausian function

     net = Kohonen_Network(20,20,3)
	 net.train(x, epochs = 4, n_width = 0.3)

![enter image description here](https://i.ibb.co/7Vp0Bfs/13.png)

We can see that there is happening a lot. Nuerons that are close together have similar weights, you can see a division into several clusters.

### Gaussian second derivative

    net = Kohonen_Network(20,20,3)
    net.train(x, epochs = 4, n_width = 7, n_fun = "second")

![enter image description here](https://i.ibb.co/zG0K00X/14.png)

Individual clusters are separated from each other by negative values, which is produced by the second derivative of the Gaussian function.

## 2.    Highly multidimensional datasets and hexagonal grid

  In this part of the report, we will conduct research on data sets with a very large number of dimensions: **MNIST** and **HAR**, they have more than half a thousand columns and several classes are specified. An additional change will be the introduction of **hexagonal** network topography and the introduction of differently calculated distances for it.

## MNIST

### Rectangular grid
#### Gaussian function

    net1 = Kohonen_Network(10,10,784)
    net1.train(X_s, epochs = 10, n_width = 0.1)


![enter image description here](https://i.ibb.co/XWTsXbQ/15.png)
 
This is the visualization of weights for each neuron after the network learning process. As we can see, clusters have been found that correspond to a particular number from the set, it can be seen that at the borders of individual groups you can notice the blurring and mixing of similar numbers. Similar numbers are close together in the network and their mapping fairly well resembles the original numbers, at least in the Gaussian neighborhood function.

![enter image description here](https://i.ibb.co/nDDRKt9/16.png)

  
In this chart we can see to which clusters the network assigns particular dataset records and how they relate to real labels. The network efficiency is quite good, and the results returned resemble the numbers they correspond to.

#### Second derivative

![enter image description here](https://i.ibb.co/QrBThCZ/17.png)

  
In the case of the second derivative of the Gaussian function, what returns the network is very little like the original data, and definitely does not reflect the appearance of numbers. You can see the division into certain clusters, they are surrounded by neurons whose values ​​have been significantly increased by the way the second derivative works.

### Hexagonal grid

  For a hexagonal topology grid, we use a different way to calculate the distance. It has been visualized in the picture below, it counts on hexagons - the nuerons correspond to the distance from hexagon 0.
  
  ![enter image description here](https://i.stack.imgur.com/tS5Yu.png)

#### Gaussian function

    net = Kohonen_Network(10,10,784, top="hex")
    net.train(X_s, epochs = 10, n_width = 0.1)

![enter image description here](https://i.ibb.co/748QPRC/18.png)

  
In this graph we see hexagons that correspond to each neuron in the network. Their color corresponds to the class that was most often assigned to a specific neuron, it is the distance from a specific record from the data to this neuron was the smallest. We see clear divisions into clusters of 9 numbers. The number 10 corresponds to a neuronon to which no points have been assigned.

![enter image description here](https://i.ibb.co/NLzzTL3/19.png)

  
This is how the visualization of scales looks. This image is a bit distorted by the shape of the grid.

![enter image description here](https://i.ibb.co/DbDTZHG/20.png)

  
And here we see the assignment to specific classes.

### Gaussian second derivative

    net = Kohonen_Network(10,10,784, top="hex")
    net.train(X_s, epochs = 10, n_width = 10, n_fun = "second")

![enter image description here](https://i.ibb.co/C1VXQr8/21.png)

Here, once again, we see more neurons that have no classes assigned to them and rather function as boundaries.

![enter image description here](https://i.ibb.co/7Rf08pc/22.png)

  
Again, the individual results returned by the network do not reflect individual numbers, but the division into certain clusters can be seen.

##  Human activity dataset

### Rectangular grid

#### Gaussian function

    net = Kohonen_Network(10,10,561)
    net.train(dataXsub, epochs = 5, n_width = 0.1)

![enter image description here](https://i.ibb.co/7rDGC3B/23.png)

In the graph above, individual cells correspond to individual neurons from the already trained network. Cell numbers are the most commonly assigned class to a particular neuron, as was the case with the previous dataset. The number 0 corresponds to neurons to which no points from the data set have been assigned. The network extracted the division into individual clusters very well, we can see a clear division into individual classes, although the data was very complicated and had a very large number of dimensions: 561.

#### Gaussian second derivative function
  


![enter image description here](https://i.ibb.co/Fg5rSjW/24.png)


  In this case, the network also well detected the division into individual clusters. No class was assigned to more neurons, as might be expected from the second derivative of the Gaussian function.


### Hexagonal grid

#### Gaussian function

    net = Kohonen_Network(10,10,561, top = "hex")
    net.train(dataXsub, epochs = 6, n_width = 0.1)



![enter image description here](https://i.ibb.co/PwtrWCj/25.png)

  
#### Gaussian second derivative function

    net = Kohonen_Network(10,10,561, top = "hex")
    net.train(dataXsub, epochs = 6, n_width = 10, n_fun = "second")

 ![enter image description here](https://i.ibb.co/TYP9tQc/26.png)
 
  
In the case of the second derivative of the Gaussian function, once again we observe that most neurons have not been assigned any class, although we can still see a good division into clusters corresponding primarily to the class from the original data set.

# Final thoughts
The above experiments show that the Kohonen network is quite a useful tool that can help us find the division into clusters. It can be especially useful with multidimensional data, when representation in two dimensions in the form of a rectangular  or hexagonal grid can help us visualize the division into groups, which would be impossible for the original number of dimensions. The tested two neighborhood functions worked in quite a different way, although both were divided into clusters, the first of them - Gaussian function, returned weights that corresponded to the original weight from the data set, which facilitated visualization. The second derivative of the Gaussian function often returned results that were significantly different from the original data, and also used fewer neurons to allocate clusters, and used more to create boundaries. It can also be caused by the implementation method, chosen architecture or parameters and data type.
Calculations using the network took quite a long time and grew very quickly with increasing mesh dimensions, the number of epochs had less impact. Depending on the parameter, the size and proximity of neuron weights were differently distributed and for different values ​​they were sometimes unevenly distributed among the clusters, so the selection of appropriate parameters was also important. In most cases, the network was good at finding clusters.   
The network needed very few epochs to learn, so the final calculations didn't last that long.

##   Sources of information
- http://mini.pw.edu.pl/~karwowskij/mioad/wyklad2.pdf - lectures
- https://github.com/tcosmo/tcosmo.github.io/blob/master/assets/soms/code/SOM_viz.ipynb - idea for visualisation
- https://books.google.pl/books?id=-fQGKI8ke_cC&pg=PA76&lpg=PA76&dq=som+second+derivative+gaussian&source=bl&ots=mIXex-tGMG&sig=ACfU3U2tHFMHlOWq28loSzx0-kFCEXDktA&hl=en&sa=X&ved=2ahUKEwjDm5WsvonpAhVKkMMKHSD8A20Q6AEwCHoECAwQAQ#v=snippet&q=sigma&f=false - second derivative idea

- https://stackoverflow.com/questions/24766079/hexagonal-grid-flat-top-distance-calculation distance visualisation

"Potwierdzam samodzielność powyższej pracy oraz niekorzystanie przeze mnie z niedozwolonych źródeł"
