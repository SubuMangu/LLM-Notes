# Introduction to PyTorch
## Components of PyTorch

<p align="center"><img src="Images/Screenshot 2025-03-15 210736.png" width="" height=""></p>

- There are three most common libraries of Pytorch:
1. **Tensor Library(`torch.tensor`):** To implement arrarys suitable for both CPUs and GPUs unlike Numpy,which is only compatible for CPU's.

2. **Automatic Differentiation Engine or Autograd(`torch.autograd`):** enables the automatic
 computation of gradients for tensor operations, simplifying backpropagation and
 model optimization.
3. **Deep Learning Library(`torch.nn`):** It offers modular, flexible, and efficient building blocks, including pretrained models, loss functions, and optimizers, for designing and training a wide range of deep learning models, catering to both researchers and developers.To create a class related to this use will use `torch.nn.Module` as parent.

## Understanding Tensors
-  It supports both CPUs and GPUs.
-  We can create objects of PyTorch’s tensor class
 using the `torch.tensor` function as shown in the following listing.

<p align="center"><img src="Images/Screenshot 2025-03-15 212007.png" width="" height=""></p>

**Tensor Data Types**
- PyTorch adopts the default **64-bit integer data** type from Python.
``` python
tensor1d = torch.tensor([1, 2, 3])
print(tensor1d.dtype)
```
torch.int64
- If we create tensors from Python floats, PyTorch creates tensors with a 32-bit precision
 by default:

``` python
floatvec = torch.tensor([1.0, 2.0, 3.0])
print(floatvec.dtype)
```
torch.float32
- A 32-bit floating-point number offers sufficient precision for most deep learning
 tasks while consuming less memory and computational resources than a 64-bit floating
point number. Moreover, GPU architectures are optimized for 32-bit computations, and
 using this data type can significantly speed up model training and inference.
- It is possible to change the precision using a tensor’s `.to` method.
- The following code demonstrates this by changing a 64-bit integer tensor into a 32-bit float tensor:
``` python
floatvec = tensor1d.to(torch.float32)
print(floatvec.dtype)
```
torch.float32

**Common PyTorch tensor operations**
1. `.shape`: allows us to access the shape of a tensor
``` python
tensor2d = torch.tensor([[1, 2, 3],
                         [4, 5, 6]])
print(tensor2d.shape)
```
torch.Size([2, 3])
- It means the tensor has two rows and three columns.

2. `.reshape`: To reshape the tensor into a $3 × 2$ tensor, we can use the `.reshape` method: 
``` python
print(tensor2d.view(3, 2))
```
tensor([[1, 2],
        [3, 4],
        [5, 6]])

3. `.view `:Same operation as `.reshape`.
- The subtle difference between `.view()` and `.reshape()` in PyTorch lies in
 their handling of memory layout: `.view()` requires the original data to be contiguous
 and will fail if it isn’t, whereas `.reshape()` will work regardless, copying the data if necessary to ensure the desired shape.

4. `.T`: to transpose a tensor
``` python
print(tensor2d.T)
```
tensor([[1, 4],
 [2, 5],
 [3, 6]])

5. `.matmul` and `@`:multiply two matrices
``` python
print(tensor2d.matmul(tensor2d.T))
```
or 
``` python
print(tensor2d @ tensor2d.T)
```
tensor([[14, 32],
 [32, 77]])

## Automatic differentiation engine
- **Automatic differentiation** in PyTorch means that it computes derivatives at runtime automatically, without requiring you to manually differentiate functions.
- Before calculating the derivatives it creates a **computational graph** first.

**Computational graph**
- A computational graph is a directed graph that allows us to express and visualize
 mathematical expressions.

<p align="center"><img src="Images/Screenshot 2025-03-16 103943.png" width="" height=""></p>

<p align="center"><img src="Images/Screenshot 2025-03-16 104106.png" width="" height=""></p>

- We will need this to compute the required gradients for backpropagation, the main
 training algorithm for neural networks as we will see in the next section.

**Automatic differentiation made easy**
- Even thoughwe don't have to find the differentiation manually ,but we have to manually set an attribute `requires_grad=True` to calculate its derivatives.
- `.backward()`: Calculates derative by backpropagating using chain rule 
- `.grad`:To find the gradient of a tensor

``` python
import torch

# Define a tensor with requires_grad=True
x = torch.tensor(2.0, requires_grad=True)

# Define a function y = x^2
y = x ** 2  

# Compute the derivative dy/dx
y.backward()

# Print the gradient
print(x.grad)  # Output: tensor(4.)
```
tensor(4.)
- If `x = torch.tensor(2.0)`, We will get error in the code, since by default `requires_grad=False`.

<p align="center"><img src="Images/Screenshot 2025-03-16 120408.png" width="" height=""></p>

- If we want to find differentiate manually then we will use `grad` function for the same as shown below
```  python
import torch.nn.functional as F
from torch.autograd import grad
y = torch.tensor([1.0])
x1 = torch.tensor([1.1])
w1 = torch.tensor([2.2], requires_grad=True)
b = torch.tensor([0.0], requires_grad=True)
z = x1 * w1 + b 
a = torch.sigmoid(z)
loss = F.binary_cross_entropy(a, y)
grad_L_w1 = grad(loss, w1, retain_graph=True)  
grad_L_b = grad(loss, b, retain_graph=True)
print(grad_L_w1)
print(grad_L_b)
```
(tensor([-0.0898]),)
(tensor([-0.0817]),)

- By default in `grad` and `backward` function , the computation graph deleted after calculating the gradients to free memory.So if we want to reuse the computation graph, we need to set `retain_graph=True`.
``` python
import torch

x = torch.tensor(2.0, requires_grad=True)
y = x ** 2  # y = x^2
y.backward()  # Computes dy/dx = 2x = 4

y.backward()  # ❌ ERROR: Graph is already deleted!
```
``` python
x = torch.tensor(2.0, requires_grad=True)
y = x ** 2  

y.backward(retain_graph=True)  # ✅ First backward pass
y.backward()  # ✅ Second backward pass (allowed)
```
## Implementing Neural Networks
-  When implementing a neural network in PyTorch, we can subclass the `torch.nn.Module`
 class to define our own custom network architecture.
-  This `Module` base class provides a
 lot of functionality, making it easier to build and train models. For instance, it allows us to
 encapsulate layers and operations and keep track of the model’s parameters. 
-  we define the network layers in the `__init__` constructor
-  we specify how the layers interact in the `forward` method. The forward method describes
 how the input data passes through the network and comes together as a computation
 graph
-  In contrast, the backward method, which we typically do not need to implement ourselves, is used during training to compute gradients of the loss function.