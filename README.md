// AutoForge - Full React App // This file will serve as the main canvas logic component // Additional files and assets will be listed below for you to build the ZIP manually or push to GitHub

import React, { useRef, useState, useEffect } from "react"; import { fabric } from "fabric";

export default function CarCustomizerApp() { const [canvasType, setCanvasType] = useState("exterior"); const [language, setLanguage] = useState("en"); const canvasRef = useRef(null);

const labels = { en: { title: "Car Customizer", upload: "Upload Image", download: "Download", addSpoiler: "Add Spoiler", addSteering: "Add Steering", red: "Red", blue: "Blue", black: "Black", interior: "Interior", exterior: "Exterior", }, hi: { title: "कार कस्टमाइज़र", upload: "इमेज अपलोड करें", download: "डाउनलोड करें", addSpoiler: "स्पॉइलर जोड़ें", addSteering: "स्टीयरिंग जोड़ें", red: "लाल", blue: "नीला", black: "काला", interior: "इंटीरियर", exterior: "एक्सटीरियर", }, };

const t = labels[language];

const initCanvas = () => { canvasRef.current = new fabric.Canvas("car-canvas", { height: 500, width: 800, backgroundColor: "#f3f4f6", });

fabric.Image.fromURL("/force-gurkha-5door.png", (img) => {
  img.scaleToWidth(600);
  canvasRef.current.setBackgroundImage(
    img,
    canvasRef.current.renderAll.bind(canvasRef.current)
  );
});

};

const handleImageUpload = (e) => { const file = e.target.files[0]; const reader = new FileReader(); reader.onload = () => { fabric.Image.fromURL(reader.result, (img) => { img.scaleToWidth(600); canvasRef.current.clear(); canvasRef.current.setBackgroundImage( img, canvasRef.current.renderAll.bind(canvasRef.current) ); }); }; reader.readAsDataURL(file); };

const addAccessory = (url) => { fabric.Image.fromURL(url, (img) => { img.scale(0.3); img.set({ left: 100, top: 100 }); canvasRef.current.add(img); }); };

const changeCarColor = (color) => { const bg = canvasRef.current.backgroundImage; if (bg) { const filter = new fabric.Image.filters.Tint({ color, opacity: 0.5 }); bg.filters = [filter]; bg.applyFilters(); canvasRef.current.renderAll(); } };

const downloadImage = () => { const dataURL = canvasRef.current.toDataURL({ format: "png" }); const a = document.createElement("a"); a.href = dataURL; a.download = ${canvasType}-custom-car.png; a.click(); };

useEffect(() => { initCanvas(); }, []);

return ( <div className="p-4 space-y-4"> <h1 className="text-2xl font-bold"> {t.title} – {canvasType.toUpperCase()} </h1>

<div className="flex gap-4 flex

