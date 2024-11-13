import React, { useState, useEffect } from 'react';
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Alert, AlertDescription } from "@/components/ui/alert";
import { 
  AlertDialog,
  AlertDialogAction,
  AlertDialogCancel,
  AlertDialogContent,
  AlertDialogDescription,
  AlertDialogFooter,
  AlertDialogHeader,
  AlertDialogTitle,
} from "@/components/ui/alert-dialog";
import { CheckCircle, XCircle, PartyPopper, Smile, Star, Undo2, ThumbsUp, Heart, Gift } from 'lucide-react';

// Mock database
const userDatabase = [
  { phone: "1234567890", email: "john@example.com", fullName: "John Doe" },
  { phone: "0987654321", email: "jane@example.com", fullName: "Jane Smith" }
];

// T-shirt SVG Component
const TshirtSVG = ({ color, size }) => (
  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 200 200" className="w-48 h-48 mx-auto">
    <path
      d="M60,40 L140,40 L160,60 L150,80 L140,70 L140,180 L60,180 L60,70 L50,80 L40,60 L60,40"
      fill={color}
      stroke="black"
      strokeWidth="2"
    />
    <text
      x="100"
      y="120"
      textAnchor="middle"
      fill={['yellow', 'green'].includes(color) ? 'black' : 'white'}
      fontSize="20"
    >
      {size}
    </text>
  </svg>
);

// Welcome Page Component
const WelcomePage = ({ userName, onContinue }) => {
  const [showIcons, setShowIcons] = useState(false);

  useEffect(() => {
    setShowIcons(true);
  }, []);

  return (
    <div className="text-center py-8 space-y-6">
      <div className={`transition-all duration-1000 ${showIcons ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'}`}>
        <h2 className="text-3xl font-bold mb-4">Welcome, {userName}! 🎉</h2>
        <div className="flex justify-center gap-6 mb-8">
          <PartyPopper className="w-12 h-12 text-yellow-500 animate-bounce" />
          <Star className="w-12 h-12 text-purple-500 animate-pulse" />
          <Smile className="w-12 h-12 text-green-500 animate-bounce" />
        </div>
        <p className="text-xl mb-8">Let's design your perfect T-shirt!</p>
        <Button 
          onClick={onContinue}
          className="bg-gradient-to-r from-purple-500 to-pink-500 hover:from-purple-600 hover:to-pink-600 text-white px-8 py-4 rounded-full text-lg"
        >
          Start Designing
        </Button>
      </div>
    </div>
  );
};

// Final Page Component
const FinalPage = ({ color, size }) => {
  const [showContent, setShowContent] = useState(false);
  const [showIcons, setShowIcons] = useState(false);
  const [showTshirt, setShowTshirt] = useState(false);
  const [showDetails, setShowDetails] = useState(false);

  useEffect(() => {
    setTimeout(() => setShowContent(true), 100);
    setTimeout(() => setShowIcons(true), 500);
    setTimeout(() => setShowTshirt(true), 1000);
    setTimeout(() => setShowDetails(true), 1500);
  }, []);

  return (
    <div className="py-8">
      <div className={`transition-all duration-1000 ${showContent ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'}`}>
        <h2 className="text-3xl font-bold text-center mb-6 animate-bounce">
          Congratulations! 🎉
        </h2>
        <div className={`flex justify-center gap-6 mb-8 transition-all duration-1000 ${showIcons ? 'opacity-100 scale-100' : 'opacity-0 scale-50'}`}>
          <ThumbsUp className="w-10 h-10 text-blue-500 animate-bounce" />
          <Heart className="w-10 h-10 text-red-500 animate-pulse" />
          <Gift className="w-10 h-10 text-purple-500 animate-bounce" />
        </div>
        <div className={`transition-all duration-1000 transform ${showTshirt ? 'opacity-100 scale-100' : 'opacity-0 scale-75'}`}>
          <div className="bg-gradient-to-r from-indigo-100 to-purple-100 rounded-lg p-8 shadow-lg">
            <TshirtSVG color={color} size={size} />
          </div>
        </div>
        <div className={`mt-8 text-center space-y-4 transition-all duration-1000 ${showDetails ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-5'}`}>
          <div className="inline-block bg-white rounded-full px-6 py-3 shadow-md">
            <p className="text-lg">
              Color: <span className="font-semibold capitalize">{color}</span>
            </p>
          </div>
          <div className="inline-block bg-white rounded-full px-6 py-3 shadow-md ml-4">
            <p className="text-lg">
              Size: <span className="font-semibold">{size}</span>
            </p>
          </div>
        </div>
        <div className={`mt-8 text-center transition-all duration-1000 ${showDetails ? 'opacity-100' : 'opacity-0'}`}>
          <p className="text-xl text-green-600 font-semibold">
            Your T-shirt design has been saved! 🌟
          </p>
        </div>
      </div>
    </div>
  );
};

// Main App Component
const TshirtApp = () => {
  const [step, setStep] = useState(1);
  const [formData, setFormData] = useState({
    phone: "",
    fullName: "",
    email: "",
    color: "",
    size: ""
  });
  
  const [error, setError] = useState("");
  const [success, setSuccess] = useState("");
  const [isVerified, setIsVerified] = useState(false);
  const [showFinal, setShowFinal] = useState(false);
  const [showWelcome, setShowWelcome] = useState(false);
  const [showConfirmation, setShowConfirmation] = useState(false);
  const [selectedSize, setSelectedSize] = useState("");
  const [history, setHistory] = useState([]);
  
  const colors = ["yellow", "green", "red", "blue"];
  const sizes = ["XXL", "XL", "L", "M", "S"];

  // Confirmation Dialog Component
  const ConfirmationDialog = () => (
    <AlertDialog open={showConfirmation}>
      <AlertDialogContent>
        <AlertDialogHeader>
          <AlertDialogTitle>Confirm Your Selection</AlertDialogTitle>
          <AlertDialogDescription className="space-y-4">
            <p>Please confirm your T-shirt selection:</p>
            <div className="bg-gray-50 p-4 rounded-lg">
              <div className="text-center mb-4">
                <TshirtSVG color={formData.color} size={selectedSize} />
              </div>
              <div className="grid grid-cols-2 gap-4 text-center">
                <div className="bg-white p-3 rounded-lg shadow-sm">
                  <p className="text-gray-600">Color</p>
                  <p className="font-semibold capitalize">{formData.color}</p>
                </div>
                <div className="bg-white p-3 rounded-lg shadow-sm">
                  <p className="text-gray-600">Size</p>
                  <p className="font-semibold">{selectedSize}</p>
                </div>
              </div>
            </div>
          </AlertDialogDescription>
        </AlertDialogHeader>
        <AlertDialogFooter>
          <AlertDialogCancel onClick={handleCancel}>
            Change Selection
          </AlertDialogCancel>
          <AlertDialogAction onClick={handleConfirm}>
            Confirm Selection
          </AlertDialogAction>
        </AlertDialogFooter>
      </AlertDialogContent>
    </AlertDialog>
  );
  
  const handleInputChange = (e) => {
    setFormData({
      ...formData,
      [e.target.name]: e.target.value
    });
    setError("");
  };
  
  const verifyUser = () => {
    const user = userDatabase.find(u => 
      u.phone === formData.phone && 
      u.email === formData.email
    );
    
    if (user) {
      setIsVerified(true);
      setSuccess("Verification successful!");
      setShowWelcome(true);
      setFormData(prev => ({
        ...prev,
        fullName: user.fullName
      }));
    } else {
      setError("User not found in database. Please submit a request.");
    }
  };
  
  const handleContinue = () => {
    setShowWelcome(false);
    setStep(2);
  };
  
  const submitRequest = () => {
    setSuccess("Request submitted! We'll get back to you soon.");
    setError("");
  };
  
  const selectColor = (color) => {
    setHistory([...history, { step: step, formData: { ...formData } }]);
    setFormData({
      ...formData,
      color
    });
    setStep(3);
  };
  
  const selectSize = (size) => {
    setSelectedSize(size);
    setShowConfirmation(true);
  };

  const handleConfirm = () => {
    setHistory([...history, { step: step, formData: { ...formData } }]);
    setFormData({
      ...formData,
      size: selectedSize
    });
    setShowConfirmation(false);
    setShowFinal(true);
  };

  const handleCancel = () => {
    setShowConfirmation(false);
    setSelectedSize("");
  };

  const handleUndo = () => {
    if (history.length > 0) {
      const lastState = history[history.length - 1];
      setStep(lastState.step);
      setFormData(lastState.formData);
      setHistory(history.slice(0, -1));
      
      if (showFinal) {
        setShowFinal(false);
        setSuccess("");
      }
    }
  };

  return (
    <Card className="w-full max-w-xl mx-auto">
      <CardHeader>
        <CardTitle className="text-2xl font-bold text-center">
          T-shirt Selection Game
        </CardTitle>
      </CardHeader>
      <CardContent>
        <ConfirmationDialog />

        {error && (
          <Alert variant="destructive" className="mb-4">
            <XCircle className="h-4 w-4" />
            <AlertDescription>{error}</AlertDescription>
          </Alert>
        )}
        
        {success && !showWelcome && !showFinal && (
          <Alert className="mb-4 bg-green-50">
            <CheckCircle className="h-4 w-4 text-green-600" />
            <AlertDescription className="text-green-600">{success}</AlertDescription>
          </Alert>
        )}
        
        {showWelcome ? (
          <WelcomePage 
            userName={formData.fullName}
            onContinue={handleContinue}
          />
        ) : showFinal ? (
          <FinalPage 
            color={formData.color}
            size={formData.size}
          />
        ) : (
          <>
            {history.length > 0 && step > 1 && (
              <div className="flex justify-end mb-4">
                <Button
                  variant="outline"
                  size="sm"
                  onClick={handleUndo}
                  className="flex items-center gap-2"
                >
                  <Undo2 className="w-4 h-4" />
                  Undo Selection
                </Button>
              </div>
            )}

            {step === 1 && (
              <div className="space-y-4">
                <Input
                  type="tel"
                  name="phone"
                  placeholder="Phone Number"
                  value={formData.phone}
                  onChange={handleInputChange}
                  className="w-full"
                />
                <Input
                  type="text"
                  name="fullName"
                  placeholder="Full Name"
                  value={formData.fullName}
                  onChange={handleInputChange}
                  className="w-full"
                />
                <Input
                  type="email"
                  name="email"
                  placeholder="Email"
                  value={formData.email}
                  onChange={handleInputChange}
                  className="w-full"
                />
                <div className="flex gap-4">
                  <Button onClick={verifyUser} className="flex-1">
                    Verify
                  </Button>
                  <Button onClick={submitRequest} variant="outline" className="flex-1">
                    Submit Request
                  </Button>
                </div>
              </div>
            )}
            
            {step === 2 && (
              <div className="space-y-4">
                <h3 className="text-lg font-semibold">Select T-shirt Color</h3>
                <div className="grid grid-cols-2 gap-4">
                  {colors.map(color => (
                    <Button
                      key={color}
                      onClick={() => selectColor(color)}
                      className="h-20 capitalize"
                      style={{
                        backgroundColor: color,
                        color: ['yellow', 'green'].includes(color) ? 'black' : 'white'
                      }}
                    >
                      {color}
                    </Button>
                  ))}
                </div>
              </div>
            )}
            
            {step === 3 && (
              <div className="space-y-4">
                <h3 className="text-lg font-semibold">Select T-shirt Size</h3>
                <div className="grid grid-cols-2 gap-4">
                  {sizes.map(size => (
                    <Button
                      key={size}
                      onClick={() => selectSize(size)}
                      variant="outline"
                      className="h-16"
                    >
                      {size}
                    </Button>
                  ))}
                </div>
              </div>
            )}
          </>
        )}
      </CardContent>
    </Card>
  );
};

export default TshirtApp;