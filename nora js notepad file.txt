// Language toggle with local storage
let lang = localStorage.getItem('lang') || 'en';
function toggleLanguage() {
    lang = lang === 'en' ? 'ur' : 'en';
    localStorage.setItem('lang', lang);
    updateLanguage();
}

function updateLanguage() {
    document.querySelectorAll('[data-en][data-ur]').forEach(elem => {
        elem.textContent = lang === 'en' ? elem.getAttribute('data-en') : elem.getAttribute('data-ur');
    });
}

// Cart functionality
let cart = JSON.parse(localStorage.getItem('cart')) || [];
function addToCart(itemName, price) {
    cart.push({ name: itemName, price: price, quantity: 1 });
    localStorage.setItem('cart', JSON.stringify(cart));
    alert(`${itemName} added to cart!`);
}

function getPrice() {
    const size = document.getElementById('size').value;
    const basePrice = 2500;
    return basePrice + (size === 'XL' ? 200 : 0); // Add ₹200 for XL
}

function updatePrice() {
    const newPrice = getPrice();
    document.querySelector('.product-info p:nth-child(2)').textContent = `Price: ₹${newPrice}`;
}

// Initialize
document.addEventListener('DOMContentLoaded', () => {
    updateLanguage();
    document.querySelector('button[onclick="toggleLanguage()"]').addEventListener('click', toggleLanguage);
    if (document.getElementById('size')) {
        document.getElementById('size').addEventListener('change', updatePrice);
        updatePrice(); // Set initial price
    }
});

// Accessibility: Trap focus (placeholder for modals)
function trapFocus(element) {
    const focusable = element.querySelectorAll('a[href], button, input, select, [tabindex]:not([tabindex="-1"])');
    const first = focusable[0];
    const last = focusable[focusable.length - 1];

    element.addEventListener('keydown', e => {
        if (e.key === 'Tab') {
            if (e.shiftKey && document.activeElement === first) {
                e.preventDefault();
                last.focus();
            } else if (!e.shiftKey && document.activeElement === last) {
                e.preventDefault();
                first.focus();
            }
        }
    });
}
