Character recognition has been applied widely for many aspects in our daily life,many systems and classification algorithms that can be able to achieve the recognition of the text have been 【proposed.But the recognition rate of handwritten characters and the large character set of text is still not high enough.In recent years,the artificial neural networks become a powerful of pattern recognition.So,how to 【conduct research in the field of character recongnition with the neural networks becomes very important.
BP neural network is a typical method of pattern recongnition for mapping the relationship of given input and output data pairs,and the relationship can be obtained through training/learning processes,nay more,BP neural network is still able to make very good predictions even the input data hava not studied.These propertise make it a powerful method for pattern recongnition and prediction in an extremely broad field.Comparing to BP neural network,the structure of convolution neural network is more complex and has more network layers.However,because of its properties of sparse connectivity,shared weights and other characteristics,it does not increase the difficulty of training and learning too much,on the contrary,its ability to identify can be improved greatly.In addition,the parallel connection of convolution neural network makes it especially suitable for image recognition.Using both of the two neural network for character recognition has a great practical and research value.
Usually we can use the image as a medium of text to achive the character recognition.【Under this premise,the image processing will affect the success of recognition directly.It should be noted that there are many random noises during the image processing in the text which should be removed or smoothed,【apart from that,the position of the text which should be found before recognition,and the size of picture and picture color channel should be normalized.Only after these pretreatments the picture then can be 【composed into a training set for the neural network.Because BP neural network cannot use the image as an input directly,it also needs us to extract characteristic parameters of the text from the picture.In this article we use the word complexity index and moment invariants as the character's feature,and the characteristic parameter extraction method has been improved.
This paper describes the 【derivation and computing process of the network structure of BP neural network and convolution neural network.【On the basis,we use both of these two networks to identify lower case letters and Chinese characters which included printed and handwritten text.Through analysis the results of the two networks for recognition,wo point out these two networks' advantages and disadvantages.At last,we compared the recognition rate of convolution neural network and the function of Adobe Acrobat's OCR and it shows that the convolution neural network has more advantages compared with BP neural network and the function of Adobe Acrobat's OCR on the character recognition.
【google translate
字符识别已经广泛应用于日常生活系统中的许多方面，并且能够实现文本识别的分类算法已经被【提出。但是，手写字符的识别率和文字的大字符集仍然不是足够高。近年来，人工神经网络成为模式识别的强大之处。因此，如何在神经网络中进行字符识别领域的研究变得非常重要。
BP神经网络是典型的模式识别方法，用于映射给定的输入和输出数据对的关系，并且可以通过训练/学习过程获得关系，此外，BP神经网络甚至可以做出非常好的预测输入数据没有被研究。这些特性使其成为一个在广泛领域的模式识别和预测的有力方法。与BP神经网络相比，卷积神经网络的结构更加复杂，网络层数更多，但由于其属性稀疏连接，共享权重等特点，不会增加训练和学习的难度，相反，其识别能力可以大大提高。此外，卷积神经网络的并行连接使其特别适用于图像识别。使用两个神经网络进行字符识别具有很大的实践和研究价值。
通常我们可以使用图像作为文本的媒介来获取字符识别。【在这个前提下，图像处理将直接影响识别的成功。应该注意的是文本中的图像处理过程中有很多随机噪声应该删除或平滑，【除此之外，在识别之前应该找到的文本的位置，以及图片和图片颜色通道的大小应该被归一化。只有在这些预处理之后，图片才可以被组合成训练集为神经网络。由于BP神经网络不能直接使用图像作为输入，还需要从图像中提取文本的特征参数。在本文中，我们使用单词复杂度索引和矩不变量作为字符特征，特征参数提取方法得到改进。
本文介绍了BP神经网络和卷积神经网络的网络结构的推导和计算过程;在此基础上，我们使用这两个网络来识别小写字母和汉字，其中包括打印和手写文本。通过分析两个网络的结果进行识别，指出了这两个网络的优缺点。最后，我们比较了卷积神经网络的识别率和Adobe Acrobat的OCR的功能，表明卷积神经网络有更多的优点与BP神经网络相比，Adobe Acrobat的OCR功能对字符识别。
