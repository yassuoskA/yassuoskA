{
 "cells": [
  {
   "cell_type": "markdown",
   "id": "cd968bd8-32d9-42e6-9189-a212c4f88908",
   "metadata": {},
   "source": [
    "# Examples\n",
    "\n",
    "## Example 3\n",
    "\n",
    "This example is a nonlinear case. Extracted from:\n",
    "\n",
    "- Brebbia, C. A., & Ferrante, A. J. (2013). Computational hydraulics. Butterworth-Heinemann.\n",
    "- Example figure 4.27.\n",
    "\n",
    "From the following relationship between velocity and height:\n",
    "\n",
    "$$ V = 0.3545 c D^{0.63} \\frac{h^{0.54}}{L^{0.54}}$$\n",
    "\n",
    "the equation is written in terms of flow rate and solved for:\n",
    "\n",
    "$$ Q = \\left(0.2784 \\frac{c D^{2.63}}{L^{0.54}} \\right) \\Delta H^{0.54} $$\n",
    "\n",
    "thus obtaining a nonlinear relationship between flow rate and height difference.\n",
    "\n",
    "$$ Q = k \\Delta H^{0.54} $$\n",
    "\n",
    "Organizing the equation conveniently for its solution:\n",
    "\n",
    "$$ Q^i = \\frac{k^i}{\\left(\\Delta H^i\\right)^{0.46}} \\Delta H ^i \\quad \\quad k^i = 0.2784 \\frac{c D^{2.63}}{L^{0.54}}$$ \n",
    "\n",
    "Writing the element matrix, according to the previous relationship:\n",
    "\n",
    "$$\\begin{bmatrix}Q_{k}^{i}\\\\Q_{j}^{i}\\end{bmatrix}\n",
    "=\\overline{k}^{i}\\begin{bmatrix}1 & -1 \\\\-1 & 1\\end{bmatrix}\n",
    "\\begin{bmatrix}H_{k}^{i}\\\\H_{j}^{i}\\end{bmatrix}$$\n",
    "\n",
    "where:\n",
    "\n",
    "$$ \\overline{k}^i = \\frac{k^i}{\\left(\\Delta H^i\\right)^{0.46}} \\quad k^i = 0.2784 \\frac{c D^{2.63}}{L^{0.54}}$$\n",
    "\n",
    "\n",
    "- $c_0 = 0.0311 \\, m^3 s^{-1}$\n",
    "- $c_1 = 0.09 \\, m^3 s^{-1}$\n",
    "- $c_2 = 0.0113 \\, m^3 s^{-1}$\n",
    "- $c_3 = 0.011 \\, m^3 s^{-1}$\n",
    "- $c_4 = 0.0101 \\, m^3 s^{-1}$\n",
    "- $c_5 = 0.02 \\, m^3 s^{-1}$\n",
    "- $c_7 = 0.015 \\, m^3 s^{-1}$\n",
    "- $D_0 = 0.1524 \\, m$\n",
    "- $D_1 = 0.1524 \\, m$\n",
    "- $D_2 = 0.1524 \\, m$\n",
    "- $D_3 = 0.1270 \\, m$\n",
    "- $D_4 = 0.1016 \\, m$\n",
    "- $D_5 = 0.1270 \\, m$\n",
    "- $D_6 = 0.1016 \\, m$\n",
    "- $D_7 = 0.1524 \\, m$\n",
    "- $D_8 = 0.1270\\, m$\n",
    "- $D_9 = 0.0127 \\, m$\n",
    "- $D_{10} = 0.1016 \\, m$\n",
    "- $D_{11} = 0.1524 \\, m$"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "c6d11eb2-f2c5-43ec-aff1-60494c8c65e0",
   "metadata": {},
   "source": [
    "The problem is defined in the same way as the previous example:"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "id": "73fafb94-5262-48b8-b977-cb102125f0c4",
   "metadata": {},
   "outputs": [],
   "source": [
    "import numpy as np\n",
    "import netsimulation as ns"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "id": "7c7c1e74-63dc-49e8-9701-d9a8af83168e",
   "metadata": {
    "tags": []
   },
   "outputs": [],
   "source": [
    "mynet = ns.NetSimulation()\n",
    "mynet.n_elements = 12\n",
    "mynet.n_nodes = 8\n",
    "\n",
    "mynet.cxy = np.array([[0, 0],\n",
    "                       [0, 500],\n",
    "                       [250, 0],\n",
    "                       [250, 500],\n",
    "                       [550, 0],\n",
    "                       [750, 500],\n",
    "                       [950, -100],\n",
    "                       [850, 200]])\n",
    "\n",
    "mynet.connectivity = np.array([[0, 1], \n",
    "                                 [0, 2],\n",
    "                                 [1, 2],\n",
    "                                 [1, 3],\n",
    "                                 [2, 3],\n",
    "                                 [2, 4],\n",
    "                                 [3, 4],\n",
    "                                 [3, 5],\n",
    "                                 [4, 5],\n",
    "                                 [4, 6],\n",
    "                                 [6, 7],\n",
    "                                 [5, 7]])\n",
    "\n",
    "mynet.calculate_length_coordinates()\n",
    "\n",
    "mynet.nodes_x_known = np.array([6])\n",
    "mynet.values_x_known = np.array([0.])\n",
    "mynet.nodes_b_known = np.array([0, 1, 2, 3, 4, 5, 7])\n",
    "mynet.values_b_known = np.array([-0.0311, \n",
    "                                0.09, \n",
    "                                -0.0113, \n",
    "                                -0.011, \n",
    "                                -0.0101, \n",
    "                                0.02, \n",
    "                                -0.015])"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "47affa5c-85ef-469a-9065-db89959e5b93",
   "metadata": {},
   "source": [
    "As it is a nonlinear problem, an initial value of $\\mathbf{k}^i$ is defined. The previously defined value of $k^i$ is used:"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "id": "755f6226-4147-4f0f-bcdd-e5468fa641fc",
   "metadata": {
    "tags": []
   },
   "outputs": [],
   "source": [
    "\n",
    "diam = np.array([0.1016, \n",
    "                 0.1524, \n",
    "                 0.1524, \n",
    "                 0.1270, \n",
    "                 0.1016, \n",
    "                 0.1270, \n",
    "                 0.1016, \n",
    "                 0.1524, \n",
    "                 0.1270, \n",
    "                 0.1016, \n",
    "                 0.1016, \n",
    "                 0.1524])\n",
    "\n",
    "chw = 110\n",
    "\n",
    "mynet.k_element = 0.2784 * chw  * diam[:]**2.63 / (mynet.length[:]**0.54)"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "aec32c65-4a34-4092-bc17-2cb8a074ee21",
   "metadata": {},
   "source": [
    "The k_element attribute will be used as an initial estimate of the solution to the problem, but it must be recalculated within the optimization algorithm. NetSimulation requires a function for this, where the input data are the vector of unknowns $\\mathbf{x}$ and the object of the NetSimulation class. Below is the implementation of the calculation of $\\mathbf{k}^i$:"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "id": "05ffd8a4-9870-4c92-85d7-f748fb172920",
   "metadata": {
    "tags": []
   },
   "outputs": [],
   "source": [
    "def k_calc(x, mynet):\n",
    "    dx = np.zeros(mynet.n_elements)\n",
    "    for i in range(mynet.n_elements):\n",
    "        c0 = mynet.connectivity[i,0]\n",
    "        c1 = mynet.connectivity[i,1]\n",
    "        dx[i] = x[c0] - x[c1]   \n",
    "    chw = 110\n",
    "    diam = np.array([0.1016, \n",
    "                     0.1524, \n",
    "                     0.1524, \n",
    "                     0.1270, \n",
    "                     0.1016, \n",
    "                     0.1270, \n",
    "                     0.1016, \n",
    "                     0.1524, \n",
    "                     0.1270, \n",
    "                     0.1016, \n",
    "                     0.1016, \n",
    "                     0.1524])\n",
    "    k = 0.2784 * chw  * diam[:]**2.63 / (mynet.length[:]**0.54) / (np.abs(dx)**0.46)\n",
    "    return k"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "55c6cb33-5ba3-4c55-a322-6ea9ee603a31",
   "metadata": {},
   "source": [
    "The object of the NetSimulation class enters the function, so it can access all its attributes easily. Inside the function, it is observed that the vector $\\mathbf{k}^i$ is recalculated where the nonlinear term of the equation $\\Delta H^{0.46}$ is considered, in this case as np.abs(dx)**0.46. Then, it is necessary to indicate to the NetSimulation object that this function will be used to calculate $\\mathbf{k}^i$ as shown below:"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "id": "c99034e6-ec59-49d1-ac4c-071430196a02",
   "metadata": {
    "tags": []
   },
   "outputs": [],
   "source": [
    "mynet.k_function = k_calc"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "789d2aa2-8032-4783-8835-0c38d3466202",
   "metadata": {},
   "source": [
    "Then the problem is solved. You can select the solver error tolerance using the `solver_tol_nonlinear` attribute, select the optimization algorithm with the `solver_nonlinear` attribute, and then call the `solve_nonlinear` method of the class:"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "id": "406ef909-d543-4e39-abb0-857edca96526",
   "metadata": {
    "tags": []
   },
   "outputs": [],
   "source": [
    "mynet.solver_tol_nonlinear = 1e-6\n",
    "mynet.solver_nonlinear = \"L-BFGS-B\"\n",
    "mynet.solve_nonlinear()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "id": "affaa91a-1a80-40f1-acf2-d5e19f59f732",
   "metadata": {
    "tags": []
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "[26.34576855 51.47421991 28.38736907 29.49233694 23.7644302  25.37018919\n",
      "  0.         17.06146977]\n",
      "[-0.0311     0.09      -0.0113    -0.011     -0.0101     0.02\n",
      " -0.0315011 -0.015    ]\n",
      "[-0.01488806 -0.01621097  0.03889733  0.03621539 -0.00275523  0.01414087\n",
      "  0.0061661   0.01629428 -0.00582486  0.01603142 -0.01546968  0.03046918]\n",
      "[-25.12845136  -2.04160053  23.08685084  21.98188297  -1.10496787\n",
      "   4.62293887   5.72790674   4.12214775  -1.60575899  23.7644302\n",
      " -17.06146977   8.30871942]\n"
     ]
    }
   ],
   "source": [
    "print(mynet.x)\n",
    "print(mynet.b)\n",
    "print(mynet.Q)\n",
    "print(mynet.dx)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "id": "727e167d-56af-4abf-9850-304d670f3744",
   "metadata": {
    "tags": []
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array([500.        , 250.        , 559.01699437, 250.        ,\n",
       "       500.        , 300.        , 583.09518948, 500.        ,\n",
       "       538.51648071, 412.31056256, 316.22776602, 316.22776602])"
      ]
     },
     "execution_count": 8,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "mynet.length"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "id": "d3e681db-f460-4dbd-889b-72331b558e2a",
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3 (ipykernel)",
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
   "version": "3.11.5"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 5
}

<!---
yassuoskA/yassuoskA is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
