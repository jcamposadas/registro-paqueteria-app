import { useState } from "react";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Select, SelectItem } from "@/components/ui/select";
import { Card, CardContent } from "@/components/ui/card";

export default function App() {
  const [empresa, setEmpresa] = useState("");
  const [operador, setOperador] = useState("");
  const [direcciones, setDirecciones] = useState([]);
  const [fotoID, setFotoID] = useState(null);
  const [fotoPlaca, setFotoPlaca] = useState(null);
  const calles = ["AMATISTA", "CORAL", "ESMERALDA", "GRANATE", "JADE", "ONIX", "RUBI", "TANZANITA", "TOPACIO", "TURQUESA", "ZAFIRO"];

  const agregarDireccion = () => {
    if (direcciones.length < 20) {
      setDirecciones([...direcciones, ""]);
    }
  };

  const actualizarDireccion = (index, value) => {
    const nuevasDirecciones = [...direcciones];
    nuevasDirecciones[index] = value;
    setDirecciones(nuevasDirecciones);
  };

  const capturarFoto = (setFoto) => {
    const fileInput = document.createElement("input");
    fileInput.type = "file";
    fileInput.accept = "image/*";
    fileInput.onchange = (e) => setFoto(URL.createObjectURL(e.target.files[0]));
    fileInput.click();
  };

  const enviarWhatsApp = () => {
    const mensaje = `Registro:\nEmpresa: ${empresa}\nOperador: ${operador}\nDirecciones: ${direcciones.join(", ")}`;
    const url = `https://wa.me/?text=${encodeURIComponent(mensaje)}`;
    window.open(url, "_blank");
  };

  return (
    <div className="p-6 max-w-xl mx-auto space-y-4">
      <h1 className="text-xl font-bold">Registro de Paquetería</h1>
      <Select value={empresa} onChange={(e) => setEmpresa(e.target.value)}>
        {["FedEx", "Amazon", "Mercado Libre", "DHL"].map((op) => (
          <SelectItem key={op} value={op}>
            {op}
          </SelectItem>
        ))}
      </Select>
      <Input placeholder="Nombre del Operador" value={operador} onChange={(e) => setOperador(e.target.value)} />
      <Button onClick={() => capturarFoto(setFotoID)}>Capturar Foto de Identificación</Button>
      {fotoID && <img src={fotoID} alt="ID" className="w-24 h-24 mt-2" />}
      <Button onClick={() => capturarFoto(setFotoPlaca)}>Capturar Foto de Placa</Button>
      {fotoPlaca && <img src={fotoPlaca} alt="Placa" className="w-24 h-24 mt-2" />}
      <div>
        {direcciones.map((direccion, index) => (
          <Select key={index} value={direccion} onChange={(e) => actualizarDireccion(index, e.target.value)}>
            {calles.map((calle) => (
              <SelectItem key={calle} value={calle}>
                {calle}
              </SelectItem>
            ))}
          </Select>
        ))}
        <Button onClick={agregarDireccion} disabled={direcciones.length >= 20}>
          Agregar Dirección
        </Button>
      </div>
      <Button className="w-full" onClick={enviarWhatsApp}>Guardar y Enviar Registro por WhatsApp</Button>
    </div>
  );
}
