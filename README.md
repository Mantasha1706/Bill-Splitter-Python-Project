class Expense:
    def __init__(self, title, paidBy, amount):
        self.paidFor = []
        self.title = title
        self.paidBy = paidBy
        self.amount = amount

    def showExpense(self):
        print()
        print(self.title)
        print(str(self.amount)+"Rs paid by", self.paidBy, "for", self.paidFor)
        print()

print("\n\n   ====================  WELCOME TO BILL SPLITTER  ====================\n")

while True:
    try:
        n = int(input("          ENTER NUMBER OF PARTICIPANTS : "))
        break
    except ValueError:
        print("\n            !!! ENTER VALID NUMBER !!!\n")



if n == -1:

    participants = ["Person1","Person2","Person3","Person4"]

    participantMap = dict()
    participantMapAlter = dict()

    for i in range(len(participants)):
        participantMap[i] = participants[i]
        participantMapAlter[participants[i]] = i

    print("Participants : ",participants)
    print("ParticipantMap : ",participantMap)
    print("ParticipantMapAlter : ",participantMapAlter)

    expenses = []

    for i in range(len(participants)):
        expenses.append(Expense("Expense"+str(i+1),None,0))

    expenses[0].amount = 300
    expenses[0].paidBy = participants[0]
    expenses[0].paidFor.append(participants[0])
    expenses[0].paidFor.append(participants[1])

    expenses[1].amount = 726
    expenses[1].paidBy = participants[1]
    expenses[1].paidFor.append(participants[0])
    expenses[1].paidFor.append(participants[1])
    expenses[1].paidFor.append(participants[2])
    expenses[1].paidFor.append(participants[3])

    expenses[2].amount = 640
    expenses[2].paidBy = participants[2]
    expenses[2].paidFor.append(participants[0])
    expenses[2].paidFor.append(participants[1])
    expenses[2].paidFor.append(participants[2])
    expenses[2].paidFor.append(participants[3])
    
    expenses[3].amount = 120
    expenses[3].paidBy = participants[3]
    expenses[3].paidFor.append(participants[0])
    expenses[3].paidFor.append(participants[1])
    expenses[3].paidFor.append(participants[2])
    expenses[3].paidFor.append(participants[3])

    print("EXPENSES : ")
    for i in expenses:
        i.showExpense()

    transactionGraph = []

    for i in range(len(participants)):
        transactionGraph.append([])
        for j in range(len(participants)):
            transactionGraph[i].append(0)

    # for i in range(len(participants)):
    #     for j in transactionGraph[i]:
    #         print(j, end = " ")
    #     print()

    for i in expenses:
        for j in i.paidFor:
            transactionGraph[participantMapAlter.get(j)][participantMapAlter.get(i.paidBy)] += (i.amount)/len(i.paidFor)

    print("\n\nBEGINNING TRANSACTION GRAPH :")
    for i in transactionGraph:
        print(i)

    for i in range(len(transactionGraph)):
        for j in range(i+1):
            if transactionGraph[i][j] > transactionGraph[j][i]:
                transactionGraph[j][i] = transactionGraph[i][j] - transactionGraph[j][i]
                transactionGraph[i][j] = 0
            else:
                transactionGraph[i][j] = transactionGraph[j][i] - transactionGraph[i][j]
                transactionGraph[j][i] = 0

    print("\n\nTRANSACTION GRAPH AFTER REDUCTION : ")
    for i in transactionGraph:
        print(i)
 
    totalAmount = []

    for i in range(len(participants)):
        totalAmount.append(0)

    for i in range(len(transactionGraph)):
        spent = 0
        toRecieve = 0
        for j in range(len(transactionGraph[i])):
            spent += transactionGraph[i][j]
            toRecieve += transactionGraph[j][i]
        totalAmount[i] = spent - toRecieve

    print("\n\nAMOUNT :", totalAmount)

    print("\n\nSUGGESTED TRANSACTIONS INORDER TO SETTLE AMOUNTS")

    settledParticipantsCount = 0

    for i in totalAmount:
        if i == 0:
            settledParticipantsCount += 1

    while True:

        if settledParticipantsCount == len(participants):
            break

        minIndex = 0
        maxIndex = 0

        for i in range(1,len(totalAmount)):
            if totalAmount[i] < totalAmount[minIndex]:
                minIndex = i
            elif totalAmount[i] > totalAmount[maxIndex]:
                maxIndex = i

        absMinIndex = 0

        if abs(totalAmount[minIndex]) > abs(totalAmount[maxIndex]):
            absMinIndex = maxIndex
        else:
            absMinIndex = minIndex

        newMinIndexAmount = totalAmount[minIndex] + abs(totalAmount[absMinIndex])
        newMaxIndexAmount = totalAmount[maxIndex] - abs(totalAmount[absMinIndex])

        transfer = abs(totalAmount[absMinIndex])

        totalAmount[minIndex] = newMinIndexAmount
        totalAmount[maxIndex] = newMaxIndexAmount

        if transfer != 0:
            print(participantMap.get(minIndex) + " must pay " + participantMap.get(maxIndex) + " an amount of INR" + str(transfer))

        settledParticipantsCount += 1
    
else:

    participant = dict()

    for i in range(n):
        participant[i] = input("\n\n     ENTER NAME OF THE PARTICIPANT : ")

    transactionGraph = []
    for i in range(len(participant)):
        transactionGraph.append([])    
        for j in range(len(participant)):
            transactionGraph[i].append(0)

    expenses = []

    flag = True
    
    while flag:

        print("\n\n     1. ADD EXPENSE.")
        print("     2. EXIT\n")

        while True:
            try:
                x = int(input("          ENTER CHOICE : "))
                break
            except ValueError:
                print("\n            !!! ENTER VALID NUMBER !!!\n")

        if x == 1:

            for i in range(1, n+1):
                print("               ", i , "." , participant.get(i-1))

            try:
                payer = int(input("          ENTER THE PERSON WHO PAYS : "))
                amount = int(input("          ENTER THE TOTAL AMOUNT PAID : "))
                receive = input("          ENTER RECEIVER SEPERATED BY COMA : ")
                temptitle = input("          ENTER TITLE FOR PAYMENT : ")
            except ValueError:
                print("\n            !!! ENTER VALID NUMBER !!!\n")
            
            arr = receive.split(",")
            temp =Expense( temptitle, participant.get(payer-1), amount )
            
            for i in range(len(arr)):
                transactionGraph[int(arr[i])-1][payer-1] += amount/len(arr)
                temp.paidFor.append(participant.get(int(arr[i])-1))

            expenses.append(temp)

        elif x == 2:
            flag = False
            break
        
        else:
            print("\n            !!! ENTER VALID NUMBER !!!\n")
    
    print("EXPENSES : ")
    for i in expenses:
        i.showExpense()

    for i in range(len(transactionGraph)):
        for j in range(i+1):
            if transactionGraph[i][j] > transactionGraph[j][i]:
                transactionGraph[j][i] = transactionGraph[i][j] - transactionGraph[j][i]
                transactionGraph[i][j] = 0
            else:
                transactionGraph[i][j] = transactionGraph[j][i] - transactionGraph[i][j]
                transactionGraph[j][i] = 0

    totalAmount = []

    for i in range(len(participant)):
        totalAmount.append(0)

    for i in range(len(transactionGraph)):
        spent = 0
        toRecieve = 0
        for j in range(len(transactionGraph[i])):
            spent += transactionGraph[i][j]
            toRecieve += transactionGraph[j][i]
        totalAmount[i] = spent - toRecieve

    print("\n\nAMOUNT :", totalAmount)

    print("\n\nSUGGESTED TRANSACTIONS INORDER TO SETTLE AMOUNTS")

    settledParticipantsCount = 0

    for i in totalAmount:
        if i == 0:
            settledParticipantsCount += 1

    while True:

        if settledParticipantsCount == len(participant):
            break

        minIndex = 0
        maxIndex = 0

        for i in range(1,len(totalAmount)):
            if totalAmount[i] < totalAmount[minIndex]:
                minIndex = i
            elif totalAmount[i] > totalAmount[maxIndex]:
                maxIndex = i

        absMinIndex = 0

        if abs(totalAmount[minIndex]) > abs(totalAmount[maxIndex]):
            absMinIndex = maxIndex
        else:
            absMinIndex = minIndex

        newMinIndexAmount = totalAmount[minIndex] + abs(totalAmount[absMinIndex])
        newMaxIndexAmount = totalAmount[maxIndex] - abs(totalAmount[absMinIndex])

        transfer = abs(totalAmount[absMinIndex])

        totalAmount[minIndex] = newMinIndexAmount
        totalAmount[maxIndex] = newMaxIndexAmount

        if transfer != 0:
            print(participant.get(minIndex) + " must pay " + participant.get(maxIndex) + " an amount of INR" + str(transfer))

        settledParticipantsCount += 1
