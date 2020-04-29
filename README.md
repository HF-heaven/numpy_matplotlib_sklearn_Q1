{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Numpy, Matplotlib and Sklearn Tutorial"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "We often use numpy to handle high dimensional arrays.\n",
    "\n",
    "Let's try the basic operation of numpy:"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {
    "scrolled": true
   },
   "outputs": [],
   "source": [
    "import numpy as np\n",
    "\n",
    "a = np.array([[1,2,3], [2,3,4]])\n",
    "print(a.ndim, a.shape, a.size, a.dtype, type(a))\n",
    "\n",
    "b = np.zeros((3,4))\n",
    "c = np.ones((3,4))\n",
    "d = np.random.randn(2,3)\n",
    "e = np.array([[1,2], [2,3], [3,4]])\n",
    "f = b*2 - c*3\n",
    "g = 2*c*f\n",
    "h = np.dot(a,e)\n",
    "i = d.mean()\n",
    "j = d.max(axis=1)\n",
    "k = a[-1][:2]\n",
    "\n",
    "# You can print from a to k for details"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "matplotlib.pyplot provides very useful apis for drawing graphs.\n",
    "\n",
    "Let's try the basic operation of matplotlib.pyplot:"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "import matplotlib.pyplot as plt\n",
    "\n",
    "x = np.arange(2, 10, 0.2)\n",
    "\n",
    "plt.plot(x, x**1.5*.5, 'r-', x, np.log(x)*5, 'g--', x, x, 'b.')\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "If you want to print them in different graphs, try this:"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "def f(x):\n",
    "    return np.sin(np.pi*x)\n",
    "\n",
    "x1 = np.arange(0, 5, 0.1)\n",
    "x2 = np.arange(0, 5, 0.01)\n",
    "\n",
    "plt.subplot(211)\n",
    "plt.plot(x1, f(x1), 'go', x2, f(x2-1))\n",
    "\n",
    "plt.subplot(212)\n",
    "plt.plot(x2, f(x2), 'r--')\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "How about printing images?\n",
    "\n",
    "Let's try to print a image whose pixels gradually change:\n",
    "\n",
    "Different pixel values represent different gray levels."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {
    "scrolled": true
   },
   "outputs": [],
   "source": [
    "img = np.arange(0, 1, 1/32/32) # define an 1D array with 32x32 elements gradually increasing\n",
    "img = img.reshape(32, 32) # reshape it into 32x32 array, the array represents a 32x32 image,\n",
    "                          # each element represents the corresponding pixel of the image\n",
    "plt.imshow(img, cmap='gray')\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "collapsed": true
   },
   "source": [
    "Based on numpy, Scikit-learn (sklearn) provides a lot of tools for machine learning.It is a very powerful machine learning library.\n",
    "\n",
    "Then, let's use it for mnist classification:"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {
    "scrolled": false
   },
   "outputs": [],
   "source": [
    "from sklearn.datasets import fetch_openml\n",
    "\n",
    "# download and load mnist data from https://www.openml.org/d/554\n",
    "# for this tutorial, the data have been downloaded already in './scikit_learn_data'\n",
    "X, Y = fetch_openml('mnist_784', version=1, data_home='./scikit_learn_data', return_X_y=True)\n",
    "\n",
    "# make the value of pixels from [0, 255] to [0, 1] for further process\n",
    "X = X / 255.\n",
    "\n",
    "# print the first image of the dataset\n",
    "img1 = X[0].reshape(28, 28)\n",
    "plt.imshow(img1, cmap='gray')\n",
    "plt.show()\n",
    "\n",
    "# print the images after simple transformation\n",
    "img2 = 1 - img1\n",
    "plt.imshow(img2, cmap='gray')\n",
    "plt.show()\n",
    "\n",
    "img3 = img1.transpose()\n",
    "plt.imshow(img3, cmap='gray')\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "# split data to train and test (for faster calculation, just use 1/10 data)\n",
    "from sklearn.model_selection import train_test_split\n",
    "X_train, X_test, Y_train, Y_test = train_test_split(X[::10], Y[::10], test_size=1000)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "collapsed": true
   },
   "source": [
    "#### Q1:\n",
    "Please use the logistic regression(default parameters) in sklearn to classify the data above, and print the training accuracy and test accuracy."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {
    "scrolled": true
   },
   "outputs": [],
   "source": [
    "# TODO:use logistic regression\n",
    "from sklearn.linear_model import LogisticRegression\n",
    "from sklearn import metrics\n",
    "from sklearn.metrics import accuracy_score\n",
    "clf = LogisticRegression()\n",
    "clf.fit(X_train,Y_train)\n",
    "predictions1 = []\n",
    "predictions2 = []\n",
    "for i in range(6000):\n",
    "    output1 = clf.predict([X_train[i]])\n",
    "    predictions1.append(output1)\n",
    "for i in range(1000):\n",
    "    output2 = clf.predict([X_test[i]])\n",
    "    predictions2.append(output2)\n",
    "train_accuracy=accuracy_score(Y_train[0:6000],predictions1)\n",
    "test_accuracy=accuracy_score(Y_test[0:1000],predictions2)\n",
    "print('Training accuracy: %0.2f%%' % (train_accuracy*100))\n",
    "print('Testing accuracy: %0.2f%%' % (test_accuracy*100))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#### Q2:\n",
    "Please use the naive bayes(Bernoulli, default parameters) in sklearn to classify the data above, and print the training accuracy and test accuracy."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "# TODO:use naive bayes\n",
    "from sklearn.naive_bayes import BernoulliNB\n",
    "from sklearn.metrics import accuracy_score\n",
    "clf = BernoulliNB()\n",
    "clf.fit(X_train,Y_train)\n",
    "predictions1 = []\n",
    "predictions2 = []\n",
    "for i in range(6000):\n",
    "    output1 = clf.predict([X_train[i]])\n",
    "    predictions1.append(output1)\n",
    "for i in range(1000):\n",
    "    output2 = clf.predict([X_test[i]])\n",
    "    predictions2.append(output2)\n",
    "train_accuracy=accuracy_score(Y_train[0:6000],predictions1)\n",
    "test_accuracy=accuracy_score(Y_test[0:1000],predictions2)\n",
    "print('Training accuracy: %0.2f%%' % (train_accuracy*100))\n",
    "print('Testing accuracy: %0.2f%%' % (test_accuracy*100))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#### Q3:\n",
    "Please use the support vector machine(default parameters) in sklearn to classify the data above, and print the training accuracy and test accuracy."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "# TODO:use support vector machine\n",
    "from sklearn.svm import LinearSVC\n",
    "from sklearn.metrics import accuracy_score\n",
    "clf = LinearSVC()\n",
    "clf.fit(X_train,Y_train)\n",
    "predictions1 = []\n",
    "predictions2 = []\n",
    "for i in range(6000):\n",
    "    output1 = clf.predict([X_train[i]])\n",
    "    predictions1.append(output1)\n",
    "for i in range(1000):\n",
    "    output2 = clf.predict([X_test[i]])\n",
    "    predictions2.append(output2)\n",
    "train_accuracy = accuracy_score(Y_train[0:6000], predictions1)\n",
    "test_accuracy = accuracy_score(Y_test[0:1000], predictions2)\n",
    "print('Training accuracy: %0.2f%%' % (train_accuracy*100))\n",
    "print('Testing accuracy: %0.2f%%' % (test_accuracy*100))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#### Q4:\n",
    "Please adjust the parameters of SVM to increase the testing accuracy, and print the training accuracy and test accuracy."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "# TODO:use SVM with another group of parameters\n",
    "from sklearn.svm import LinearSVC\n",
    "from sklearn.metrics import accuracy_score\n",
    "clf = LinearSVC(C=2.0,max_iter=3000)\n",
    "clf.fit(X_train,Y_train)\n",
    "predictions1 = []\n",
    "predictions2 = []\n",
    "for i in range(6000):\n",
    "    output1 = clf.predict([X_train[i]])\n",
    "    predictions1.append(output1)\n",
    "for i in range(1000):\n",
    "    output2 = clf.predict([X_test[i]])\n",
    "    predictions2.append(output2)\n",
    "train_accuracy = accuracy_score(Y_train[0:6000], predictions1)\n",
    "test_accuracy = accuracy_score(Y_test[0:1000], predictions2)\n",
    "print('Training accuracy: %0.2f%%' % (train_accuracy*100))\n",
    "print('Testing accuracy: %0.2f%%' % (test_accuracy*100))"
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.7.4"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
