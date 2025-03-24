import React, { useState, useRef, Suspense } from 'react';
import { Canvas, useFrame } from '@react-three/fiber';
import { OrbitControls, Stars, Float } from '@react-three/drei';
import * as THREE from 'three';

// 3D Skill Sphere Component
function SkillSphere({ skill, color }) {
  const meshRef = useRef();
  const [active, setActive] = useState(false);

  useFrame(({ clock }) => {
    if (meshRef.current) {
      meshRef.current.rotation.x += 0.01;
      meshRef.current.rotation.y += 0.01;
    }
  });

  return (
    <Float
      speed={1.5}
      rotationIntensity={0.5}
      floatIntensity={0.5}
    >
      <mesh
        ref={meshRef}
        scale={active ? 1.5 : 1}
        onClick={() => setActive(!active)}
        onPointerOver={() => setActive(true)}
        onPointerOut={() => setActive(false)}
      >
        <sphereGeometry args={[0.5, 32, 32]} />
        <meshStandardMaterial 
          color={color} 
          opacity={0.8} 
          transparent 
          emissive={color}
          emissiveIntensity={0.5}
        />
        <Suspense fallback={null}>
          <Html
            position={[0, 1, 0]}
            distanceFactor={10}
            center
          >
            <div className="text-white text-xs">{skill}</div>
          </Html>
        </Suspense>
      </mesh>
    </Float>
  );
}

// Main Portfolio Component
function Portfolio() {
  const skills = [
    { name: 'React', color: '#61DAFB' },
    { name: 'Laravel', color: '#FF2D20' },
    { name: 'Node.js', color: '#339933' },
    { name: 'MongoDB', color: '#4EA94B' },
    { name: 'Docker', color: '#2CA5E0' }
  ];

  return (
    <div className="h-screen w-full bg-gradient-to-br from-blue-900 via-purple-900 to-black">
      <Canvas 
        camera={{ position: [0, 0, 5], fov: 60 }}
        className="absolute inset-0"
      >
        <ambientLight intensity={0.5} />
        <pointLight position={[10, 10, 10]} />
        
        {/* Background Stars */}
        <Stars 
          radius={300} 
          depth={50} 
          count={5000} 
          factor={4} 
          saturation={0} 
        />

        {/* Skill Spheres */}
        <group position={[0, 0, 0]}>
          {skills.map((skill, index) => (
            <SkillSphere 
              key={skill.name}
              skill={skill.name}
              color={skill.color}
              position={[
                Math.cos(index * Math.PI * 2 / skills.length) * 2,
                Math.sin(index * Math.PI * 2 / skills.length) * 2,
                0
              ]}
            />
          ))}
        </group>

        {/* Orbit Controls */}
        <OrbitControls enableZoom={true} />
      </Canvas>

      {/* Overlay Text */}
      <div className="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 text-center text-white">
        <h1 className="text-5xl font-bold mb-4 animate-pulse">
          Charaf Eddine
        </h1>
        <p className="text-2xl text-blue-300 animate-fade-in">
          Full Stack Web Developer
        </p>
      </div>
    </div>
  );
}

export default Portfolio;
