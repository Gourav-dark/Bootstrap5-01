using System;
using System.Collections.Generic;
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

namespace ECommerceApp.Models
{
    public class UserRole
    {
        public int Id { get; set; }
        [Required]
        [MaxLength(50)]
        public string RoleName { get; set; }
        public DateTime CreatedAt { get; set; } = DateTime.UtcNow;
        public DateTime? UpdatedAt { get; set; }
        
        public ICollection<User> Users { get; set; }
    }

    public class User
    {
        public int Id { get; set; }
        public int RoleId { get; set; }
        [MaxLength(100)]
        public string FullName { get; set; }
        [Required]
        [MaxLength(100)]
        public string Email { get; set; }
        [Required]
        [MaxLength(15)]
        public string PhoneNumber { get; set; }
        [Required]
        [MaxLength(20)]
        public string Status { get; set; } = "inactive";
        public DateTime? PhoneVerifiedAt { get; set; }
        public DateTime? EmailVerifiedAt { get; set; }
        public DateTime CreatedAt { get; set; } = DateTime.UtcNow;
        public DateTime? UpdatedAt { get; set; }

        [ForeignKey("RoleId")]
        public UserRole UserRole { get; set; }
        public ICollection<VerificationCode> VerificationCodes { get; set; }
        public ICollection<Address> Addresses { get; set; }
        public ICollection<Cart> Carts { get; set; }
        public ICollection<Order> Orders { get; set; }
    }

    public class VerificationCode
    {
        public int Id { get; set; }
        public int UserId { get; set; }
        [MaxLength(10)]
        public string Code { get; set; }
        [Required]
        [MaxLength(20)]
        public string VerificationType { get; set; }
        [Required]
        [MaxLength(20)]
        public string Status { get; set; }
        public DateTime CreatedAt { get; set; } = DateTime.UtcNow;
        public DateTime? UpdatedAt { get; set; }

        [ForeignKey("UserId")]
        public User User { get; set; }
    }

    public class Category
    {
        public int Id { get; set; }
        [Required]
        [MaxLength(100)]
        public string CategoryName { get; set; }
        [Required]
        [MaxLength(20)]
        public string Status { get; set; } = "active";
        public DateTime CreatedAt { get; set; } = DateTime.UtcNow;
        public DateTime? UpdatedAt { get; set; }

        public ICollection<Product> Products { get; set; }
    }

    public class Product
    {
        public int Id { get; set; }
        [Required]
        [MaxLength(100)]
        public string Name { get; set; }
        public string Description { get; set; }
        [Required]
        public decimal Price { get; set; }
        public int? CategoryId { get; set; }
        public int Stock { get; set; } = 0;
        [Required]
        [MaxLength(20)]
        public string Status { get; set; } = "active";
        public DateTime CreatedAt { get; set; } = DateTime.UtcNow;
        public DateTime? UpdatedAt { get; set; }

        [ForeignKey("CategoryId")]
        public Category Category { get; set; }
        public ICollection<ProductImage> ProductImages { get; set; }
        public ICollection<ProductSupplier> ProductSuppliers { get; set; }
        public ICollection<CartItem> CartItems { get; set; }
        public ICollection<OrderItem> OrderItems { get; set; }
    }

    public class ProductImage
    {
        public int Id { get; set; }
        public int ProductId { get; set; }
        [Required]
        public string ImageUrl { get; set; }
        [Required]
        [MaxLength(20)]
        public string Status { get; set; } = "active";
        public DateTime CreatedAt { get; set; } = DateTime.UtcNow;
        public DateTime? UpdatedAt { get; set; }

        [ForeignKey("ProductId")]
        public Product Product { get; set; }
    }

    public class Supplier
    {
        public int Id { get; set; }
        [Required]
        [MaxLength(100)]
        public string Name { get; set; }
        [MaxLength(100)]
        public string ContactName { get; set; }
        [MaxLength(100)]
        public string Email { get; set; }
        [MaxLength(15)]
        public string PhoneNumber { get; set; }
        [Required]
        public string Address { get; set; }
        [Required]
        [MaxLength(100)]
        public string City { get; set; }
        [Required]
        [MaxLength(100)]
        public string State { get; set; }
        [Required]
        [MaxLength(100)]
        public string Country { get; set; }
        [Required]
        [MaxLength(10)]
        public string PostalCode { get; set; }
        [Required]
        [MaxLength(20)]
        public string Status { get; set; } = "active";
        public DateTime CreatedAt { get; set; } = DateTime.UtcNow;
        public DateTime? UpdatedAt { get; set; }

        public ICollection<ProductSupplier> ProductSuppliers { get; set; }
    }

    public class ProductSupplier
    {
        public int Id { get; set; }
        public int ProductId { get; set; }
        public int SupplierId { get; set; }
        public DateTime CreatedAt { get; set; } = DateTime.UtcNow;
        public DateTime? UpdatedAt { get; set; }

        [ForeignKey("ProductId")]
        public Product Product { get; set; }
        [ForeignKey("SupplierId")]
        public Supplier Supplier { get; set; }
    }

    public class Cart
    {
        public int Id { get; set; }
        public int UserId { get; set; }
        public DateTime CreatedAt { get; set; } = DateTime.UtcNow;
        public DateTime? UpdatedAt { get; set; }

        [ForeignKey("UserId")]
        public User User { get; set; }
        public ICollection<CartItem> CartItems { get; set; }
    }

    public class CartItem
    {
        public int Id { get; set; }
        public int CartId { get; set; }
        public int ProductId { get; set; }
        public int Quantity { get; set; }

        public DateTime CreatedAt { get; set; } = DateTime.UtcNow;
        public DateTime? UpdatedAt { get; set; }

        [ForeignKey("CartId")]
        public Cart Cart { get; set; }
        [ForeignKey("ProductId")]
        public Product Product { get; set; }
    }

    public class Order
    {
        public int Id { get; set; }
        public int UserId { get; set; }
        public decimal TotalAmount { get; set; }
        public decimal DiscountAmount { get; set; }
        [Required]
        [MaxLength(30)]
        public string PaymentMode { get; set; }
        [Required]
        [MaxLength(20)]
        public string PaymentStatus { get; set; }
        [Required]
        [MaxLength(20)]
        public string Status { get; set; } = "placed";
        public DateTime CreatedAt { get; set; } = DateTime.UtcNow;
        public DateTime? UpdatedAt { get; set; }

        [ForeignKey("UserId")]
        public User User { get; set; }
        public ICollection<OrderItem> OrderItems { get; set; }
    }

    public class OrderItem
    {
        public int Id { get; set; }
        public int OrderId { get; set; }
        public int ProductId { get; set; }
        public int Quantity { get; set; }
        public decimal Price { get; set; }
        public DateTime CreatedAt { get; set; } = DateTime.UtcNow;
        public DateTime? UpdatedAt { get; set; }

        [ForeignKey("OrderId")]
        public Order Order { get; set; }
        [ForeignKey("ProductId")]
        public Product Product { get; set; }
    }

    public class Address
    {
        public int Id { get; set; }
        public int UserId { get; set; }
        [Required]
        public string AddressLine { get; set; }
        public string Locality { get; set; }
        public string City { get; set; }
        public string State { get; set; }
        public string Country { get; set; }
        public string PostalCode { get; set; }
        public DateTime CreatedAt { get; set; } = DateTime.UtcNow;
        public DateTime? UpdatedAt { get; set; }

        [ForeignKey("UserId")]
        public User User { get; set; }
    }

    public class PaymentTransaction
    {
        public int Id { get; set; }
        public int OrderId { get; set; }
        public decimal Amount { get; set; }
        [Required]
        [MaxLength(30)]
        public string PaymentMode { get; set; }
        public string PaymentResponse { get; set; }
        [Required]
        [MaxLength(20)]
        public string PaymentStatus { get; set; }
        public DateTime CreatedAt { get; set; } = DateTime.UtcNow;
        public DateTime? UpdatedAt { get; set; }

        [ForeignKey("OrderId")]
        public Order Order { get; set; }
    }

    public class Offer
    {
        public int Id { get; set; }
        [Required]
        [MaxLength(20)]
        public string CouponCode { get; set; }
        [Required]
        [MaxLength(20)]
        public string DiscountType { get; set; }
        public decimal DiscountValue { get; set; }
        public decimal MaxDiscountAmount { get; set; }
        public DateTime StartDate { get; set; }
        public DateTime EndDate { get; set; }
        public string Description { get; set; }
        [Required]
        [MaxLength(20)]
        public string Status { get; set; } = "active";
        public DateTime CreatedAt { get; set; } = DateTime.UtcNow;
        public DateTime? UpdatedAt { get; set; }
        
        public ICollection<OrderDiscount> OrderDiscounts { get; set; }
    }

    public class OrderDiscount
    {
        public int Id { get; set; }
        public int OrderId { get; set; }
        public int OfferId { get; set; }
        public DateTime CreatedAt { get; set; } = DateTime.UtcNow;
        public DateTime? UpdatedAt { get; set; }

        [ForeignKey("OrderId")]
        public Order Order { get; set; }
        [ForeignKey("OfferId")]
        public Offer Offer { get; set; }
    }
}
