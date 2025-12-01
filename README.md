<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Luxe Models - Tienda Exclusiva</title>
    
    <!-- Tailwind CSS (Estilos) -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- React y ReactDOM (Motor de la app) -->
    <script crossorigin src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    
    <!-- Babel (Para entender el código moderno) -->
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

    <!-- Configuraciones de Tailwind -->
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        neutral: { 850: '#1f1f1f', 900: '#171717', 950: '#0a0a0a' },
                        rose: { 400: '#fb7185', 500: '#f43f5e', 600: '#e11d48' }
                    },
                    animation: {
                        'fade-in': 'fadeIn 0.5s ease-out',
                        'slide-up': 'slideUp 0.5s ease-out',
                        'slide-left': 'slideLeft 0.3s ease-out',
                    },
                    keyframes: {
                        fadeIn: { '0%': { opacity: '0' }, '100%': { opacity: '1' } },
                        slideUp: { '0%': { transform: 'translateY(20px)', opacity: '0' }, '100%': { transform: 'translateY(0)', opacity: '1' } },
                        slideLeft: { '0%': { transform: 'translateX(100%)' }, '100%': { transform: 'translateX(0)' } }
                    }
                }
            }
        }
    </script>
    <style>
        /* Estilos base para scrollbar oscura */
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { bg: #171717; }
        ::-webkit-scrollbar-thumb { background: #333; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #444; }
    </style>
</head>
<body class="bg-neutral-900 text-white font-sans antialiased">
    <div id="root"></div>

    <script type="text/babel">
        // --- ICONOS SVG (Para no depender de librerías externas) ---
        const Icon = ({ path, size = 24, className = "" }) => (
            <svg xmlns="http://www.w3.org/2000/svg" width={size} height={size} viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className={className}>
                {path}
            </svg>
        );

        const Icons = {
            Camera: (props) => <Icon {...props} path={<><path d="M14.5 4h-5L7 7H4a2 2 0 0 0-2 2v9a2 2 0 0 0 2 2h16a2 2 0 0 0 2-2V9a2 2 0 0 0-2-2h-3l-2.5-3z"/><circle cx="12" cy="13" r="3"/></>} />,
            ShoppingCart: (props) => <Icon {...props} path={<><circle cx="9" cy="21" r="1"/><circle cx="20" cy="21" r="1"/><path d="M1 1h4l2.68 13.39a2 2 0 0 0 2 1.61h9.72a2 2 0 0 0 2-1.61L23 6H6"/></>} />,
            X: (props) => <Icon {...props} path={<><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></>} />,
            ArrowLeft: (props) => <Icon {...props} path={<><line x1="19" y1="12" x2="5" y2="12"/><polyline points="12 19 5 12 12 5"/></>} />,
            Star: (props) => <Icon {...props} path={<polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"/>} />,
            Info: (props) => <Icon {...props} path={<><circle cx="12" cy="12" r="10"/><line x1="12" y1="16" x2="12" y2="12"/><line x1="12" y1="8" x2="12.01" y2="8"/></>} />,
            Trash2: (props) => <Icon {...props} path={<><polyline points="3 6 5 6 21 6"/><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"/><line x1="10" y1="11" x2="10" y2="17"/><line x1="14" y1="11" x2="14" y2="17"/></>} />,
            ExternalLink: (props) => <Icon {...props} path={<><path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/><polyline points="15 3 21 3 21 9"/><line x1="10" y1="14" x2="21" y2="3"/></>} />
        };

        // --- DATOS DE LA TIENDA ---
        const INITIAL_DATA = [
            {
                id: 1,
                name: "Sofía",
                tagline: "Estilo Urbano & Fashion",
                bio: "Hola, soy Sofía. Me especializo en fotografía de moda urbana y estilo de vida. Mis paquetes incluyen sesiones exclusivas en locaciones de la ciudad.",
                image: "https://images.unsplash.com/photo-1494790108377-be9c29b29330?q=80&w=1000&auto=format&fit=crop",
                packages: [
                    { id: 's1', name: "Pack Básico", price: 200, description: "5 Fotografías digitales HD + 1 Video corto." },
                    { id: 's2', name: "Pack Premium", price: 500, description: "15 Fotografías 4K + Acceso a contenido exclusivo." },
                    { id: 's3', name: "Sesión VIP", price: 1200, description: "Acceso completo a la sesión del mes + Chat personalizado." }
                ]
            },
            {
                id: 2,
                name: "Valentina",
                tagline: "Glamour & Estudio",
                bio: "Bienvenidos a mi mundo. El arte y la elegancia son mi pasión. Encuentra aquí mis mejores trabajos editoriales y de estudio.",
                image: "https://images.unsplash.com/photo-1534528741775-53994a69daeb?q=80&w=1000&auto=format&fit=crop",
                packages: [
                    { id: 'v1', name: "Silver Set", price: 300, description: "Colección de 10 fotos artísticas." },
                    { id: 'v2', name: "Gold Set", price: 650, description: "Colección completa de 30 fotos + Detrás de cámaras." }
                ]
            },
            {
                id: 3,
                name: "Isabella",
                tagline: "Fitness & Lifestyle",
                bio: "Motivación y disciplina. Comparte conmigo mi viaje fitness a través de sesiones exclusivas de entrenamiento y modelaje deportivo.",
                image: "https://images.unsplash.com/photo-1531746020798-e6953c6e8e04?q=80&w=1000&auto=format&fit=crop",
                packages: [
                    { id: 'i1', name: "Starter Pack", price: 150, description: "3 Fotos exclusivas fitness." },
                    { id: 'i2', name: "Elite Pack", price: 450, description: "Galería completa (20 fotos) + Video de rutina." },
                    { id: 'i3', name: "Ultra Fan", price: 1500, description: "Contenido personalizado a petición + Colección completa." }
                ]
            }
        ];

        const PHONE_NUMBER = "524774834664"; 

        // --- COMPONENTE PRINCIPAL ---
        const { useState, useEffect } = React;

        function App() {
            const [view, setView] = useState('home');
            const [selectedModel, setSelectedModel] = useState(null);
            const [cart, setCart] = useState([]);
            const [isCartOpen, setIsCartOpen] = useState(false);

            useEffect(() => {
                const savedCart = localStorage.getItem('modelStoreCart');
                if (savedCart) setCart(JSON.parse(savedCart));
            }, []);

            useEffect(() => {
                localStorage.setItem('modelStoreCart', JSON.stringify(cart));
            }, [cart]);

            const addToCart = (pkg, modelName) => {
                const newItem = {
                    cartId: Date.now(),
                    packageId: pkg.id,
                    packageName: pkg.name,
                    price: pkg.price,
                    modelName: modelName
                };
                setCart([...cart, newItem]);
                setIsCartOpen(true);
            };

            const removeFromCart = (cartId) => {
                setCart(cart.filter(item => item.cartId !== cartId));
            };

            const getTotalPrice = () => {
                return cart.reduce((total, item) => total + item.price, 0);
            };

            const handleCheckout = () => {
                if (cart.length === 0) return;
                let message = `Hola, estoy interesado en comprar el siguiente contenido:\n\n`;
                cart.forEach(item => {
                    message += `🔹 *${item.modelName}* - ${item.packageName} ($${item.price})\n`;
                });
                message += `\n💰 *Total a pagar: $${getTotalPrice()}*\n\nEspero instrucciones.`;
                const whatsappUrl = `https://wa.me/${PHONE_NUMBER}?text=${encodeURIComponent(message)}`;
                window.open(whatsappUrl, '_blank');
            };

            const handleModelClick = (model) => {
                setSelectedModel(model);
                setView('model');
                window.scrollTo(0, 0);
            };

            const goHome = () => {
                setView('home');
                setSelectedModel(null);
            };

            return (
                <div className="min-h-screen pb-20">
                    
                    {/* Header */}
                    <header className="fixed top-0 w-full z-50 bg-neutral-900/90 backdrop-blur-md border-b border-neutral-800 shadow-lg">
                        <div className="max-w-5xl mx-auto px-4 h-16 flex items-center justify-between">
                            <div onClick={goHome} className="flex items-center gap-2 cursor-pointer hover:opacity-80 transition">
                                <Icons.Camera className="text-rose-500" size={24} />
                                <h1 className="text-xl font-bold tracking-tight">LUXE <span className="text-rose-500">MODELS</span></h1>
                            </div>
                            
                            <button onClick={() => setIsCartOpen(true)} className="relative p-2 hover:bg-neutral-800 rounded-full transition">
                                <Icons.ShoppingCart size={24} />
                                {cart.length > 0 && (
                                    <span className="absolute top-0 right-0 bg-rose-500 text-white text-[10px] font-bold w-5 h-5 flex items-center justify-center rounded-full animate-bounce">
                                        {cart.length}
                                    </span>
                                )}
                            </button>
                        </div>
                    </header>

                    <main className="pt-24 max-w-5xl mx-auto px-4">
                        {/* Home View */}
                        {view === 'home' && (
                            <div className="animate-fade-in">
                                <div className="text-center mb-12 space-y-2">
                                    <h2 className="text-3xl md:text-5xl font-bold bg-gradient-to-r from-white to-neutral-400 bg-clip-text text-transparent">
                                        Contenido Exclusivo
                                    </h2>
                                    <p className="text-neutral-400 max-w-lg mx-auto">
                                        Selecciona una modelo para ver sus paquetes disponibles.
                                    </p>
                                </div>
                                <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
                                    {INITIAL_DATA.map((model) => (
                                        <div key={model.id} onClick={() => handleModelClick(model)} className="group relative rounded-2xl overflow-hidden cursor-pointer bg-neutral-800 border border-neutral-700 hover:border-rose-500/50 transition-all duration-300 hover:shadow-2xl hover:shadow-rose-500/10 hover:-translate-y-1">
                                            <div className="aspect-[3/4] overflow-hidden">
                                                <img src={model.image} alt={model.name} className="w-full h-full object-cover group-hover:scale-110 transition duration-700" />
                                                <div className="absolute inset-0 bg-gradient-to-t from-black/90 via-black/20 to-transparent opacity-90"></div>
                                            </div>
                                            <div className="absolute bottom-0 left-0 w-full p-6">
                                                <h3 className="text-2xl font-bold mb-1 flex items-center gap-2">
                                                    {model.name} <Icons.Star size={16} className="text-rose-500 fill-rose-500" />
                                                </h3>
                                                <p className="text-rose-400 text-sm font-medium mb-2">{model.tagline}</p>
                                                <button className="w-full py-2 bg-white/10 backdrop-blur-sm rounded-lg text-sm font-semibold hover:bg-rose-600 transition flex items-center justify-center gap-2">
                                                    Ver Paquetes <Icons.ArrowLeft size={16} className="rotate-180" />
                                                </button>
                                            </div>
                                        </div>
                                    ))}
                                </div>
                            </div>
                        )}

                        {/* Model View */}
                        {view === 'model' && selectedModel && (
                            <div className="animate-slide-up">
                                <button onClick={goHome} className="mb-6 flex items-center gap-2 text-neutral-400 hover:text-white transition">
                                    <Icons.ArrowLeft size={20} /> Volver al catálogo
                                </button>
                                <div className="grid grid-cols-1 md:grid-cols-3 gap-8">
                                    <div className="md:col-span-1 space-y-6">
                                        <div className="aspect-[3/4] rounded-2xl overflow-hidden border border-neutral-700 shadow-2xl">
                                            <img src={selectedModel.image} alt={selectedModel.name} className="w-full h-full object-cover" />
                                        </div>
                                        <div className="bg-neutral-800/50 p-6 rounded-2xl border border-neutral-700">
                                            <h2 className="text-2xl font-bold mb-2">{selectedModel.name}</h2>
                                            <p className="text-neutral-400 text-sm leading-relaxed">{selectedModel.bio}</p>
                                        </div>
                                    </div>
                                    <div className="md:col-span-2">
                                        <h3 className="text-2xl font-bold mb-6 flex items-center gap-2">
                                            Paquetes Disponibles <Icons.Info size={18} className="text-neutral-500" />
                                        </h3>
                                        <div className="space-y-4">
                                            {selectedModel.packages.map((pkg) => (
                                                <div key={pkg.id} className="bg-neutral-800 border border-neutral-700 rounded-xl p-6 flex flex-col md:flex-row justify-between items-start md:items-center gap-4 hover:border-rose-500 transition-colors">
                                                    <div className="flex-1">
                                                        <div className="flex items-center gap-3 mb-1">
                                                            <h4 className="text-xl font-bold text-white">{pkg.name}</h4>
                                                            <span className="px-2 py-0.5 rounded text-xs font-bold bg-rose-500/20 text-rose-400 border border-rose-500/20">${pkg.price} MXN</span>
                                                        </div>
                                                        <p className="text-neutral-400 text-sm">{pkg.description}</p>
                                                    </div>
                                                    <button onClick={() => addToCart(pkg, selectedModel.name)} className="w-full md:w-auto px-6 py-3 bg-white text-black font-bold rounded-lg hover:bg-rose-500 hover:text-white transition flex items-center justify-center gap-2 whitespace-nowrap active:scale-95 transform duration-100">
                                                        Agregar <Icons.ShoppingCart size={18} />
                                                    </button>
                                                </div>
                                            ))}
                                        </div>
                                    </div>
                                </div>
                            </div>
                        )}
                    </main>

                    {/* Cart Modal */}
                    {isCartOpen && (
                        <div className="fixed inset-0 z-[60]">
                            <div className="absolute inset-0 bg-black/80 backdrop-blur-sm transition-opacity" onClick={() => setIsCartOpen(false)} />
                            <div className="absolute top-0 right-0 h-full w-full md:w-[450px] bg-neutral-900 border-l border-neutral-800 shadow-2xl flex flex-col animate-slide-left">
                                <div className="p-6 border-b border-neutral-800 flex justify-between items-center bg-neutral-900">
                                    <h2 className="text-xl font-bold flex items-center gap-2">
                                        Tu Carrito <span className="bg-rose-500 text-white text-xs px-2 py-0.5 rounded-full">{cart.length}</span>
                                    </h2>
                                    <button onClick={() => setIsCartOpen(false)} className="p-2 hover:bg-neutral-800 rounded-full transition text-neutral-400 hover:text-white">
                                        <Icons.X size={24} />
                                    </button>
                                </div>
                                <div className="flex-1 overflow-y-auto p-6 space-y-4">
                                    {cart.length === 0 ? (
                                        <div className="text-center py-20 text-neutral-500">
                                            <Icons.ShoppingCart size={48} className="mx-auto mb-4 opacity-20" />
                                            <p>Tu carrito está vacío.</p>
                                            <button onClick={() => setIsCartOpen(false)} className="mt-4 text-rose-500 hover:underline font-medium">Ver modelos</button>
                                        </div>
                                    ) : (
                                        cart.map((item) => (
                                            <div key={item.cartId} className="flex justify-between items-center bg-neutral-800/50 p-4 rounded-xl border border-neutral-800">
                                                <div>
                                                    <h4 className="font-bold text-white">{item.modelName}</h4>
                                                    <p className="text-sm text-neutral-400">{item.packageName}</p>
                                                    <p className="text-rose-400 font-bold mt-1">${item.price}</p>
                                                </div>
                                                <button onClick={() => removeFromCart(item.cartId)} className="p-2 text-neutral-500 hover:text-red-500 hover:bg-red-500/10 rounded-lg transition" title="Eliminar">
                                                    <Icons.Trash2 size={20} />
                                                </button>
                                            </div>
                                        ))
 
