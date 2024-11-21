using System;
using System.Collections.Generic;

public class DictionaryRepository<TKey, TValue> where TKey : IComparable
{
    private Dictionary<TKey, TValue> _items = new Dictionary<TKey, TValue>();

    // Add a new item with a unique identifier
    public void Add(TKey id, TValue item)
    {
        if (_items.ContainsKey(id))
        {
            throw new ArgumentException("An item with the same key already exists.");
        }
        _items[id] = item;
    }

    // Retrieve an item by its key
    public TValue Get(TKey id)
    {
        if (!_items.TryGetValue(id, out TValue item))
        {
            throw new KeyNotFoundException("Item not found.");
        }
        return item;
    }

    // Update an item with the specified key
    public void Update(TKey id, TValue newItem)
    {
        if (!_items.ContainsKey(id))
        {
            throw new KeyNotFoundException("Item not found.");
        }
        _items[id] = newItem;
    }

    // Remove an item by its key
    public void Delete(TKey id)
    {
        if (!_items.Remove(id))
        {
            throw new KeyNotFoundException("Item not found.");
        }
    }

    // List all items
    public Dictionary<TKey, TValue> GetAll()
    {
        return _items;
    }
}
public class Product
{
    public int ProductId { get; set; }
    public string ProductName { get; set; }

    public override string ToString()
    {
        return $"ProductId: {ProductId}, ProductName: {ProductName}";
    }
}
public class Program
{
    public static void Main(string[] args)
    {
        var productRepository = new DictionaryRepository<int, Product>();
        bool running = true;

        while (running)
        {
            Console.WriteLine("\nProduct Management System");
            Console.WriteLine("1. Add Product");
            Console.WriteLine("2. Get Product");
            Console.WriteLine("3. Update Product");
            Console.WriteLine("4. Delete Product");
            Console.WriteLine("5. List All Products");
            Console.WriteLine("6. Exit");
            Console.Write("Choose an option: ");

            string choice = Console.ReadLine();

            switch (choice)
            {
                case "1":
                    // Add Product
                    var newProduct = new Product();
                    Console.Write("Enter Product ID: ");
                    newProduct.ProductId = int.Parse(Console.ReadLine());
                    Console.Write("Enter Product Name: ");
                    newProduct.ProductName = Console.ReadLine();
                    try
                    {
                        productRepository.Add(newProduct.ProductId, newProduct);
                        Console.WriteLine("Product added successfully.");
                    }
                    catch (ArgumentException ex)
                    {
                        Console.WriteLine(ex.Message);
                    }
                    break;

                case "2":
                    // Get Product
                    Console.Write("Enter Product ID to retrieve: ");
                    int getId = int.Parse(Console.ReadLine());
                    try
                    {
                        var product = productRepository.Get(getId);
                        Console.WriteLine("Retrieved Product: " + product);
                    }
                    catch (KeyNotFoundException ex)
                    {
                        Console.WriteLine(ex.Message);
                    }
                    break;

                case "3":
                    // Update Product
                    Console.Write("Enter Product ID to update: ");
                    int updateId = int.Parse(Console.ReadLine());
                    var updatedProduct = new Product();
                    Console.Write("Enter new Product Name: ");
                    updatedProduct.ProductName = Console.ReadLine();
                    updatedProduct.ProductId = updateId; // Keep the same ID
                    try
                    {
                        productRepository.Update(updateId, updatedProduct);
                        Console.WriteLine("Product updated successfully.");
                    }
                    catch (KeyNotFoundException ex)
                    {
                        Console.WriteLine(ex.Message);
                    }
                    break;

                case "4":
                    // Delete Product
                    Console.Write("Enter Product ID to delete: ");
                    int deleteId = int.Parse(Console.ReadLine());
                    try
                    {
                        productRepository.Delete(deleteId);
                        Console.WriteLine("Product deleted successfully.");
                    }
                    catch (KeyNotFoundException ex)
                    {
                        Console.WriteLine(ex.Message);
                    }
                    break;

                case "5":
                    // List All Products
                    Console.WriteLine("\nAll Products:");
                    foreach (var item in productRepository.GetAll())
                    {
                        Console.WriteLine(item.Value);
                    }
                    break;

                case "6":
                    // Exit
                    running = false;
                    Console.WriteLine("Exiting the program.");
                    break;

                default:
                    Console.WriteLine("Invalid option. Please try again.");
                    break;
            }
        }
    }
}
