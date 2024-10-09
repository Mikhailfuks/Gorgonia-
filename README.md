package main

import ("fmt")

 "github.com/chewxy/gorgonia/tensor"
 "github.com/chewxy/gorgonia"
 "github.com/chewxy/gorgonia/gorgonia/ops"
)

func main() {
 // Create a Gorgonia graph
 g := gorgonia.NewGraph()

 // Define the input nodes
 x := gorgonia.NewMatrix(g, tensor.Float64, tensor.WithShape(2, 2), tensor.WithBacking(tensor.Float64s{1, 2, 3, 4}), tensor.WithName("x"))
 y := gorgonia.NewMatrix(g, tensor.Float64, tensor.WithShape(2, 2), tensor.WithBacking(tensor.Float64s{5, 6, 7, 8}), tensor.WithName("y"))

 // Define the operation
 z := ops.Add(g, x, y) // Add x and y

 // Create a machine to run the graph
 machine := gorgonia.NewMachine(g)
 defer machine.Close()

 // Run the graph
 if err := machine.RunAll(); err != nil {
  panic(err)
 }

 // Retrieve the result
 zValue, err := z.Value()
 if err != nil {
  panic(err)
 }

 // Print the result
 fmt.Println("z:", zValue)
}
