```tsx
'use client';

import { AnimatePresence } from 'framer-motion';
import React, { useEffect, useState } from 'react';
import { createPortal } from 'react-dom';

interface ModalProps {
  children: React.ReactNode;
  isOpen: boolean;
  onClose: (value: boolean) => void;
}

export default function Modal({ children, isOpen, onClose }: ModalProps) {
  const [mounted, setMounted] = useState(false);

  useEffect(() => {
    // eslint-disable-next-line react-hooks/set-state-in-effect
    setMounted(true);
  }, []);

  if (!mounted) return null;

  return createPortal(
    <AnimatePresence>
      {isOpen && (
        <div onClick={() => onClose(false)} className="fixed inset-0 z-[9999] flex items-center justify-center bg-black/20 backdrop-blur-[15px]">
          <div className="z-[10000]" onClick={(e) => e.stopPropagation()}>
            {children}
          </div>
        </div>
      )}
    </AnimatePresence>,
    document.body
  );
}
```
