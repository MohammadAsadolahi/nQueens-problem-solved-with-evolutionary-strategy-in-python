#   solving n Queens arrange without any clashes in n*n Chess board with GA(genetic algorithm) 
#   by mohammad asadolahi
#   mohmad.asa1994@gmail.com
import random
import numpy as np
from matplotlib import pyplot as plt


class Chromosome:
    def __init__(self, solution):
        self.solution = solution
        self.ClashCount = self.GetClashCount()

    def __lt__(self, other):
        return self.ClashCount < self.ClashCount

    def updateClashCount(self):
        self.ClashCount = self.GetClashCount()

    def GetClashCount(self):
        chromosomeClash = 0
        row = 0
        while row < len(self.solution):
            column = row + 1
            while column < len(self.solution):
                if (self.solution[row] == self.solution[column]) or (row - self.solution[row]) == (
                        column - self.solution[column]) or (row + self.solution[row]) == (
                        column + self.solution[column]):
                    chromosomeClash += 1
                column += 1
            row += 1
        return chromosomeClash


class GeneticSolver:
    def __init__(self, populationSize, generationCount, mutationRate, boardSize):
        self.populationSize = populationSize
        self.generationCount = generationCount
        self.mutationRate = mutationRate
        self.boardSize = boardSize
        self.population = []
        self.elitePopulation = []
        self.generationAverage = []
        self.initialPopulation()

    def initialPopulation(self):
        index = 0
        while index < self.populationSize:
            solution = []
            for number in range(0, self.boardSize):
                solution.append(random.randint(1, self.boardSize))
            if not(self.isChromosomeExist(self.population, Chromosome(solution))):
                self.population.append(Chromosome(solution))
                index += 1
        self.population.sort(key=lambda chromosome: chromosome.ClashCount)
        self.elitePopulation.append(self.population[0])
        self.generationAverage.append((sum(x.ClashCount for x in self.population)) / self.populationSize)

    def printPopulation(self):
        for chromosome in self.population:
            print(f"{chromosome.solution} with count of {chromosome.ClashCount} clashes")

    def printElitePopulation(self):
        generation = 0
        print("******************************************************************************************")
        print(f"printing elite chromosomes of all generations")
        for chromosome in self.elitePopulation:
            print(
                f"elite chromosome of generation:{generation} is: {chromosome.solution} with count of {chromosome.ClashCount} clasehs.")
            generation += 1
        print("******************************************************************************************")

    def crossOver(self, firstParent, secondParent):
        crossOverPosition = int(self.boardSize / 2)
        return firstParent.solution[0:crossOverPosition] + secondParent.solution[
                                                           crossOverPosition:len(firstParent.solution)]

    def mutate(self, population, chromosome):
        tmpChromosome = Chromosome(chromosome.solution[::])
        while self.isChromosomeExist(population, tmpChromosome):
            mutationIndex = random.randint(0, len(tmpChromosome.solution) - 1)
            mutationChange = random.randint(1, len(tmpChromosome.solution))
            tmpChromosome.solution[mutationIndex] = mutationChange
        chromosome.solution = tmpChromosome.solution[::]
        chromosome.updateClashCount()

    def mutateBySwap(self, population, chromosome):
        tmpChromosome = Chromosome(chromosome.solution[::])

        while self.isChromosomeExist(population, tmpChromosome):
            mutationIndex1 = random.randint(0, len(tmpChromosome.solution) - 1)
            mutationIndex2 = random.randint(0, len(tmpChromosome.solution) - 1)
            if(mutationIndex1!=mutationIndex2):
                temp=tmpChromosome.solution[mutationIndex1]
                tmpChromosome.solution[mutationIndex1]=tmpChromosome.solution[mutationIndex2]
                tmpChromosome.solution[mutationIndex2]=temp
        chromosome.solution = tmpChromosome.solution[::]
        chromosome.updateClashCount()

    def isChromosomeExist(self, population, chromosome):
        for gene in population:
            if gene.solution == chromosome.solution:
                return True
        return False

    def solve(self):
        self.lunchEvolution()
        plt.plot([x.ClashCount for x in self.elitePopulation], label="Elites")
        plt.xlabel('x - Generations')
        plt.ylabel('y - Clashes ')
        plt.title('Evolution of elite chromosomes')
        plt.show()

        plt.plot([x for x in self.generationAverage], label="Average clashes")
        plt.title('Averge clashes of each generatins')
        plt.xlabel('x - Generations')
        plt.ylabel('y - Clashes ')
        plt.show()

        plt.plot([x.ClashCount for x in self.elitePopulation], label="Elites")
        plt.xlabel('x - Generations')
        plt.ylabel('y - Clashes ')
        plt.title('Evolution of elite chromosomes')
        plt.legend()
        plt.plot([x for x in self.generationAverage], label="Average clashes")
        plt.xlabel('x - Generations')
        plt.ylabel('y - Clashes ')
        plt.title('Averge clashes of each generatins')
        plt.legend()
        plt.show()

        image = np.ones(self.boardSize * self.boardSize)
        image = image.reshape((self.boardSize, self.boardSize))
        line = []
        index = 0
        for i in self.elitePopulation.pop().solution:
            line = []
            for j in range(1, self.boardSize + 1):
                if i == j:
                    line.append(0)
                else:
                    line.append(1)
            image[index] = line
            index += 1
        row_labels = range(1, self.boardSize+1)
        col_labels = range(1, self.boardSize+1)
        plt.matshow(image)
        plt.xticks(range(self.boardSize), col_labels)
        plt.yticks(range(self.boardSize), row_labels)
        plt.show()

    def lunchEvolution(self):
        generation = 0
        while generation < self.generationCount:
            for chromosome in self.population:
                if chromosome.ClashCount == 0:
                    self.elitePopulation.append(chromosome)
                    print(
                        f"an absolute solution found in generation:{generation} arrange: {chromosome.solution} whith zero clash")
                    return
            print("******************************************************************************************")
            print(f"generation: {generation}")
            newPopulation = self.population[::]
            crossoverIndex = 0
            while crossoverIndex < self.populationSize:
                firstChildPprobability = random.randint(0, 100)
                secondChildPprobability = random.randint(0, 100)
                child1 = Chromosome(
                    self.crossOver(self.population[crossoverIndex], self.population[crossoverIndex + 1]))
                if (firstChildPprobability < self.mutationRate) or self.isChromosomeExist(newPopulation, child1):
                    self.mutate(newPopulation, child1)
                newPopulation.append(child1)
                child2 = Chromosome(
                    self.crossOver(self.population[crossoverIndex + 1], self.population[crossoverIndex]))
                if (secondChildPprobability < self.mutationRate) or self.isChromosomeExist(newPopulation, child2):
                    self.mutate(newPopulation, child2)
                newPopulation.append(child2)
                crossoverIndex += 2
            newPopulation.sort(key=lambda chromosome: chromosome.ClashCount)
            self.population.clear()
            self.population = newPopulation[0:self.populationSize]
            self.elitePopulation.append(self.population[0])
            self.generationAverage.append((sum(x.ClashCount for x in self.population)) / self.populationSize)
            print(
                f"the best arrange of generation: {generation} is{self.elitePopulation[generation].solution} "
                f"wit" f"h {self.elitePopulation[generation].ClashCount} clashes")
            print(
                f"the average clashes of generation: {generation} is{self.generationAverage[generation]} ")
            self.printPopulation()
            generation += 1

# populationSize, generationCount, mutationRate, boardSize
boardSolver = GeneticSolver(160,50, 10,10)
boardSolver.solve()
boardSolver.printElitePopulation()


