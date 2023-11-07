# Car inventory
class Car:
    def __init__(self, make, model, year, price):
        self.make = make.upper()
        self.model = model.upper()
        self.year = year 
        self.price = price
    def __gt__(self, rhs):
        if self.make == rhs.make:
            if self.model == rhs.model:
                if self.year == rhs.year:
                    return self.price > rhs.price
                else:
                    return self.year > rhs.year               
            else:
                return self.model > rhs.model
        else:
            return self.make > rhs.make
    def __lt__(self, rhs):
        if self.make == rhs.make:
            if self.model == rhs.model:
                if self.year == rhs.year:
                    return self.price < rhs.price
                else:
                    return self.year < rhs.year               
            else:
                return self.model < rhs.model
        else:
            return self.make < rhs.make
        
    def __eq__(self, rhs):
        if self.make == rhs.make and self.model==rhs.model and self.year==rhs.year and self.price==rhs.price:
            return True
        else:
            return False
    def __str__(self):
        x= f'Make: {self.make}, Model: {self.model}, Year: {self.year}, Price: ${self.price}'
        return x
from Car import Car

class CarInventoryNode:
    def __init__(self, car):
        self.car = car
        self.make = car.make
        self.model = car.model
        self.cars = [car]
        self.parent = None
        self.left = None
        self.right = None
    
    def getMake(self):
        return self.car.make
    
    def getModel(self):
        return self.car.model
    
    def getParent(self):
        return self.parent
    
    def setParent(self, parent):
        self.parent = parent
        
    def getLeft(self):
        return self.left
    
    def setLeft(self, left):
        self.left = left

    def getRight(self):
        return self.right
    
    def setRight(self, right):
        self.right = right
        
    def hasLeftChild(self):
        return self.left

    def hasRightChild(self):
        return self.right

    def isLeftChild(self):
        return self.parent and self.parent.left == self

    def isRightChild(self):
        return self.parent and self.parent.right == self

    def isRoot(self):
        return not self.parent

    def isLeaf(self):
        return not (self.right or self.left)

    def hasAnyChildren(self):
        return self.right or self.left

    def hasBothChildren(self):
        return self.right and self.left
    

    def replaceNodeData(self,car,cars,make,model,lc,rc):
        self.car = car
        self.cars = cars
        self.make = make
        self.model = model
        self.left = lc
        self.right = rc
        if self.hasLeftChild():
            self.left.parent = self
        if self.hasRightChild():
            self.right.parent = self 

    def __str__(self):
        result = ''
        for x in self.cars:
            result += str(x)+'\n'
        return result

from CarInventoryNode import *
from Car import *
class CarInventory:
    def __init__(self):
        self.root = None
        self.collection = []
    
    def _addCar(self, car, node):
        if car.model == node.model and car.make == node.make:
            node.cars.append(car)
        elif car < node.car:
            if node.left:
                return self._addCar(car, node.left)         
            else:
                node.setLeft(CarInventoryNode(car))
                node.left.setParent(node)
        elif car>node.car: 
            if node.right:
                return self._addCar(car, node.right)  
            else:
                node.setRight(CarInventoryNode(car))
                node.right.setParent(node)
            
    def addCar(self, car):
        self.collection.append(car)
        if self.root:
            if car.model == self.root.model and car.make == self.root.make:
                self.root.cars.append(car)
            else:
                self._addCar(car, self.root)
        else:
            self.root = CarInventoryNode(car)

    def get(self, car, node):
        if not node:
            return False
        else:
            if car.model == node.model and car.make == node.make:
                for i in node.cars:
                    if i == car:
                        return True
                return False

            elif car < node.car:
                return self.get(car, node.left)
            elif car>node.car:
                return self.get(car, node.right)
        
        
    def doesCarExist(self, car):
        if not self.root:
            return False
        else:
            if car.model == self.root.model and car.make == self.root.make:
                for i in self.root.cars:
                    if i == car:
                        return True
                return False
            elif car < self.root.car:
                return self.get(car, self.root.left)
            else:
                return self.get(car, self.root.right)
            
        
    def _preOrder(self, node):
        x = ''
        if node != None:
            x += str(node)
            x += self._inOrder(node.left)
            x += self._inOrder(node.right)
        return x

    def preOrder(self):
        x = ''
        if self.root != None:
            x += str(self.root)
            x += self._inOrder(self.root.left)
            x += self._inOrder(self.root.right)
        return x
    
    def _inOrder(self, node):
        x = ''
        if node != None:
            x += self._inOrder(node.left)
            x += str(node)
            x += self._inOrder(node.right)
        return x

    def inOrder(self):
        x = ''
        if self.root != None:
            x += self._inOrder(self.root.left)
            x += str(self.root)
            x += self._inOrder(self.root.right)
        return x

    def _postOrder(self, node):
        x = ''
        if node != None:
            x += self._inOrder(node.left)
            x += self._inOrder(node.right)
            x += str(node)
        return x

    def postOrder(self):
        x = ''
        if self.root != None:
            x += self._inOrder(self.root.left)
            x += self._inOrder(self.root.right)
            x += str(self.root)
        return x
        
    
    def getWorstCar(self, make, model):
        make = make.upper()
        model = model.upper()
        if self.root == None:
            return None
        y= []
        for i in self.collection:
            if i.make == make and i.model == model:
                y.append(i)
        if len(y) == 0:
            return None
        else:
            return min(y)
        
    def getBestCar(self, make, model):
        make = make.upper()
        model = model.upper()
        if self.root == None:
            return None
        y= []
        for i in self.collection:
            if i.make == make and i.model == model:
                y.append(i)
        if len(y) == 0:
            return None
        else:
            return max(y)
        
    def getTotalInventoryPrice(self):
        x = 0
        for i in self.collection:
            x+= i.price
        return x

    def _get(self, make, model, node):
        if not node:
            return None
        elif [node.make, node.model] == [make, model]:
            return node
        elif node.make > make or node.make == make and node.model > model:
            return self._get(make, model, node.left)
        else:
            return self._get(make, model, node.right)

    def _min(self, node):
        while node.left:
            node = node.left
        return node

    def getSuccessor(self, make, model):
        make = make.upper()
        model = model.upper()
        currentnode = self._get(make, model, self.root)
        if not currentnode:
            return
        else:
            succ = None
            if currentnode.right:
                succ = self._min(currentnode.right)
            else:
                if currentnode.parent:
                    if currentnode.isLeftChild():
                        succ = currentnode.parent
                    else:
                        while currentnode:
                            if currentnode.isLeftChild():
                                succ = currentnode.parent
                                break
                            else:
                                currentnode = currentnode.parent
            return succ

    def delete(self, y, collection):
        x = 0
        for i in collection:
            if y== i:
                del collection[x]
            x += 1

    def spliceOut(self, node):
        if node.isLeaf():
            if node.isLeftChild():
                node.parent.left = None
            else:
                node.parent.right = None
        elif node.hasAnyChildren():
            if node.hasRightChild():
                if node.isLeftChild():
                    node.parent.left= node.right
                    node.right.parent = node.parent
                else:
                    node.parent.right= node.right
                    node.right.parent = node.parent
			
    def removeCar(self, make, model, year, price):
        car = Car(make, model, year, price)
        
        if self.doesCarExist(car):
            self.delete(car, self.collection)
            currentNode = self._get(make.upper(), model.upper(), self.root)
            self.delete(car, currentNode.cars)
            
            if len(currentNode.cars) == 0:
                if currentNode.isLeaf():
                    if currentNode.parent == None:
                        self.root = None                        
                    elif currentNode == currentNode.parent.left:
                        currentNode.parent.left = None
                    else:
                            currentNode.parent.right = None
                elif currentNode.hasBothChildren():
                    succ = self.getSuccessor(make, model)
                    self.spliceOut(succ)
                    currentNode.car = succ.car
                    currentNode.cars= succ.cars
                    currentNode.make = succ.make
                    currentNode.model= succ.model
                else:
                    if currentNode.hasLeftChild():
                        if currentNode.isLeftChild():
                            currentNode.left.parent = currentNode.parent
                            currentNode.parent.left = currentNode.left
                        elif currentNode.isRightChild():
                            currentNode.left.parent = currentNode.parent
                            currentNode.parent.right = currentNode.left
                        else: 
                            currentNode.replaceNodeData(currentNode.left.car,
                                currentNode.left.cars,
                                currentNode.left.make,
                                currentNode.left.model,
                                currentNode.left.left,
                                currentNode.left.right)

                    else:
                        if currentNode.isLeftChild():
                            currentNode.right.parent = currentNode.parent
                            currentNode.parent.left = currentNode.right
                        elif currentNode.isRightChild():
                            currentNode.right.parent = currentNode.parent
                            currentNode.parent.right = currentNode.right
                        else:
                            currentNode.replaceNodeData(currentNode.right.car,
                                currentNode.right.cars,
                                currentNode.right.make,
                                currentNode.right.model,
                                currentNode.right.left,
                                currentNode.right.right)               
            return True
        else:
            return False


from Car import *
from CarInventoryNode import *
from CarInventory import *

def test_Car():
    a = Car("Benz", "CRV", 2000, 3000)
    b = Car("Benz", "CRV", 2000, 3000)
    c = Car("Mercedes-Benz", "AMG", 2000, 3000)
    assert (a == b) == True
    assert str(a) == 'Make: BENZ, Model: CRV, Year: 2000, Price: $3000'
    assert str(c) == 'Make: MERCEDES-BENZ, Model: AMG, Year: 2000, Price: $3000'

def test_CarInventoryNode():
    x= Car("TESLA", "CRV", 2001, 6000)
    y= Car("Jeep", "CRV", 2001, 4000)
    carNode = CarInventoryNode(x)
    carNode.cars.append(y)
    assert str(carNode) == 'Make: TESLA, Model: CRV, Year: 2001, Price: $6000\nMake: JEEP, Model: CRV, Year: 2001, Price: $4000\n'
    z= Car("TESLA", "SUV", 2015, 6000)
    h= Car("TESLA", "SUV", 2015, 10000)
    car = CarInventoryNode(z)
    car.cars.append(h)
    assert str(car) == 'Make: TESLA, Model: SUV, Year: 2015, Price: $6000\nMake: TESLA, Model: SUV, Year: 2015, Price: $10000\n'
    assert (car == carNode) == False
def test_CarInventory():
    bst = CarInventory()

    car1 = Car("Nissan", "Leaf", 2018, 17000)
    car2 = Car("Tesla", "Model3", 2018, 80000)
    car3 = Car("Mercedes", "Sprinter", 2022, 40000)
    car4 = Car("Mercedes", "Sprinter", 2014, 25000)
    car5 = Car("Ford", "Ranger", 2021, 25000)

    bst.addCar(car1)
    bst.addCar(car2)
    bst.addCar(car3)
    bst.addCar(car4)
    bst.addCar(car5)
    assert bst.getBestCar("Nissan", "Leaf") == car1
    assert bst.getBestCar("Mercedes", "Sprinter") == car3
    assert bst.getBestCar("Honda", "Accord") == None

    assert bst.getWorstCar("Nissan", "Leaf") == car1
    assert bst.getWorstCar("Mercedes", "Sprinter") == car4
    assert bst.getBestCar("Honda", "Accord") == None

    assert bst.getTotalInventoryPrice() == 187000
    assert bst.inOrder() == \
"""\
Make: FORD, Model: RANGER, Year: 2021, Price: $25000
Make: MERCEDES, Model: SPRINTER, Year: 2022, Price: $40000
Make: MERCEDES, Model: SPRINTER, Year: 2014, Price: $25000
Make: NISSAN, Model: LEAF, Year: 2018, Price: $17000
Make: TESLA, Model: MODEL3, Year: 2018, Price: $80000
"""
    assert bst.postOrder() == \
"""\
Make: FORD, Model: RANGER, Year: 2021, Price: $25000
Make: MERCEDES, Model: SPRINTER, Year: 2022, Price: $40000
Make: MERCEDES, Model: SPRINTER, Year: 2014, Price: $25000
Make: TESLA, Model: MODEL3, Year: 2018, Price: $80000
Make: NISSAN, Model: LEAF, Year: 2018, Price: $17000
"""
    assert bst.getSuccessor('lamborgini','star')==None
    assert bst.getSuccessor('flywolf','dream')==None
    assert bst.getSuccessor("FORD", "RANGER")==bst.root.left
    assert bst.getSuccessor("MERCEDES", "SPRINTER")==bst.root
    
    assert bst.removeCar("Tesla", "Model3", 2019, 80000)==False
    assert bst.removeCar("Tesla", "Model3", 2018, 100000)==False
    
    assert bst.removeCar("Tesla", "Model3", 2018, 80000)==True    
    assert bst.doesCarExist(car2)==False
    assert bst.removeCar("Ford", "Ranger", 2021, 25000)==True
    assert bst.doesCarExist(car5)==False
