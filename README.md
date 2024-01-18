import math
prodA = []
prodB = []
prodC = []

    
def discount():
    disc = dict()
    disc['flat_10_discount'] = -1
    disc['bulk_10_discount'] = -1
    disc['bulk_5_discount'] = -1
    disc['tiered_50_discount'] = -1
    if cart_val > 200:
        disc['flat_10_discount'] = 10
    if quantity > 20:
        disc['bulk_10_discount'] = 0.1*cart_val
    
    if prodA[0] > 10:
        disc['bulk_5_discount'] = prodA[2] * 0.05
    if prodB[0] > 10:
        disc['bulk_5_discount']+= prodB[2] * 0.05
    if prodC[0] > 10:
        disc['bulk_5_discount']+= prodC[2] * 0.05

    if quantity>30:
        disc['tiered_50_discount'] = 0
        if prodA[0] > 15:
            disc['tiered_50_discount'] = prodA[2] * 0.5
        if prodB[0] > 15:
            disc['tiered_50_discount']+= prodB[2] * 0.5
        if prodC[0] > 15:
            disc['tiered_50_discount']+= prodC[2] * 0.5
        if prodA[0] <= 15 and prodB[0] <= 15 and prodC[0] <= 15:
            l = [0]*prodA[0]
            l+= [1]*prodB[0]
            l+= [2]*prodC[0]
            if l[14] == 0:
                disc['tiered_50_discount'] = (prodB[2] + prodC[2])*0.5
            elif l[14] == 1:
                c = 0
                for i in l[14::-1]:
                    if i == 1:
                        c+=1
                disc['tiered_50_discount'] = ((prodB[0]-c)*40 + prodC[2])*0.5
            elif l[14] == 2:
                c = 0
                for i in l[14::-1]:
                    print(i)
                    if i == 2:
                        c+=1
                    if i == 1:
                        break
                disc['tiered_50_discount'] = ((prodC[0]-c)*50)*0.5        
    return disc

def ship():
    return math.ceil(quantity / 10)*5

def gift():
    units = 0
    if prodA[1] == 'Y':
        units+= prodA[0]
    if prodB[1] == 'Y':
        units+= prodB[0]
    if prodC[1] == 'Y':
        units+= prodC[0]
    return units

# [0] is quantity, [1] is gift choice, [2] is total value 
print("Enter quantity for Product A")
prodA.append(int(input()))
print("Will it be a gift? (Y/N)")
prodA.append(input())
print("Enter quantity for Product B")
prodB.append(int(input()))
print("Will it be a gift? (Y/N)")
prodB.append(input())
print("Enter quantity for Product C")
prodC.append(int(input()))
print("Will it be a gift? (Y/N)")
prodC.append(input())

quantity = prodA[0]+ prodB[0] + prodC[0]

prodA.append(prodA[0]*20)
prodB.append(prodB[0]*40)
prodC.append(prodC[0]*50)

cart_val = prodA[2] + prodB[2]+ prodC[2]
disc = discount()
disc_name = max(disc, key= lambda a: disc[a])
disc_value = max(disc.values())
total = cart_val - disc_value + ship() + gift()
print(disc)
print("Product Name \t Quantity \t Amount")
print("Product A \t",prodA[0],"\t\t",prodA[2])
print("Product B \t",prodB[0],"\t\t",prodB[2])
print("Product C \t",prodC[0],"\t\t",prodC[2])
print("Sub Total \t \t\t",cart_val)
print("Discount Applied: "+disc_name)
print("Discount Amount: \t\t",disc_value)
print("Shipping Fee \t \t\t",ship())
print("Gift Wrap Fee \t \t\t",gift())
print("Total \t \t",total)

