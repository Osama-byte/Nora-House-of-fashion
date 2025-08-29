// Redirect to billing page with product and price
const goToBilling = (product, price) => {
    const url = `billing.html?product=${encodeURIComponent(product)}&price=${encodeURIComponent(price)}`;
    window.location.href = url;
};
// Language toggle with local storage
let lang = localStorage.getItem('lang') || 'en';
function toggleLanguage() {
    lang = lang === 'en' ? 'ur' : 'en';
    localStorage.setItem('lang', lang);
    updateLanguage();
}

// Smooth scroll for navigation links
document.querySelectorAll('a[href^="#"]').forEach(link => {
    link.addEventListener('click', function (e) {
        const target = document.querySelector(this.getAttribute('href'));
        if (target) {
            e.preventDefault();
            target.scrollIntoView({ behavior: 'smooth' });
        }
    });
});

// Product quick view modal logic
function openQuickView(productId) {
    const modal = document.getElementById('quickViewModal');
    if (!modal) return;
    // Populate modal with product info (placeholder logic)
    modal.querySelector('.modal-content').textContent = `Quick view for product ${productId}`;
    modal.style.display = 'block';
    trapFocus(modal);
}
function closeQuickView() {
    const modal = document.getElementById('quickViewModal');
    if (modal) modal.style.display = 'none';
}

// FAQ toggle logic
document.querySelectorAll('.faq-question').forEach(q => {
    q.addEventListener('click', function () {
        this.classList.toggle('active');
        const answer = this.nextElementSibling;
        if (answer) answer.style.display = answer.style.display === 'block' ? 'none' : 'block';
    });
});

// Testimonial slider (basic)
let testimonialIndex = 0;
function showTestimonial(n) {
    const testimonials = document.querySelectorAll('.testimonial');
    if (!testimonials.length) return;
    testimonials.forEach(t => t.style.display = 'none');
    testimonialIndex = (n + testimonials.length) % testimonials.length;
    testimonials[testimonialIndex].style.display = 'block';
}
function nextTestimonial() { showTestimonial(testimonialIndex + 1); }
function prevTestimonial() { showTestimonial(testimonialIndex - 1); }
document.addEventListener('DOMContentLoaded', () => {
    showTestimonial(0);
    document.querySelectorAll('.testimonial-next').forEach(btn => btn.addEventListener('click', nextTestimonial));
    document.querySelectorAll('.testimonial-prev').forEach(btn => btn.addEventListener('click', prevTestimonial));
});

// Newsletter form validation
document.querySelectorAll('.newsletter-form').forEach(form => {
    form.addEventListener('submit', function (e) {
        const email = this.querySelector('input[type="email"]');
        if (email && !/^\S+@\S+\.\S+$/.test(email.value)) {
            e.preventDefault();
            alert('Please enter a valid email address.');
        }
    });
});

// Gallery filter
document.querySelectorAll('.gallery-filter button').forEach(btn => {
    btn.addEventListener('click', function () {
        const filter = this.getAttribute('data-filter');
        document.querySelectorAll('.gallery-item').forEach(item => {
            item.style.display = filter === 'all' || item.classList.contains(filter) ? 'block' : 'none';
        });
    });
});

// Animated counters
function animateCounter(el, target) {
    let count = 0;
    const step = Math.ceil(target / 100);
    function update() {
        count += step;
        if (count > target) count = target;
        el.textContent = count;
        if (count < target) requestAnimationFrame(update);
    }
    update();
}
document.querySelectorAll('.counter').forEach(el => {
    const target = parseInt(el.getAttribute('data-target'), 10);
    if (!isNaN(target)) animateCounter(el, target);
});

// Pricing table toggle (monthly/yearly)
document.querySelectorAll('.pricing-toggle').forEach(toggle => {
    toggle.addEventListener('change', function () {
        const yearly = this.checked;
        document.querySelectorAll('.pricing-amount').forEach(amount => {
            amount.textContent = yearly ? amount.getAttribute('data-yearly') : amount.getAttribute('data-monthly');
        });
    });
});

// Accessibility: keyboard skip to main content
document.addEventListener('keydown', function (e) {
    if (e.altKey && e.key === 'm') {
        const main = document.querySelector('main');
        if (main) main.focus();
    }
});

// Back to top button
const backToTop = document.getElementById('backToTop');
if (backToTop) {
    window.addEventListener('scroll', function () {
        backToTop.style.display = window.scrollY > 300 ? 'block' : 'none';
    });
    backToTop.addEventListener('click', function () {
        window.scrollTo({ top: 0, behavior: 'smooth' });
    });
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


// Modern product image hover: swap to malaysian hijab 1-4.webp
const setupProductImageHover = () => {
    const hijabImages = [
        'malaysian hijab 1.webp',
        'malaysian hijab 2.webp',
        'malaysian hijab 3.webp',
        'malaysian hijab 4.webp'
    ];
    document.querySelectorAll('.featured-products .product-card img').forEach((img, idx) => {
        img.dataset.originalSrc = img.getAttribute('src');
        img.addEventListener('mouseenter', () => {
            img.setAttribute('src', hijabImages[idx % hijabImages.length]);
        });
        img.addEventListener('mouseleave', () => {
            img.setAttribute('src', img.dataset.originalSrc);
        });
    });
};

document.addEventListener('DOMContentLoaded', () => {
    // ...existing code...
    setupProductImageHover();
});
