### text

```tsx
export default function CtrlElement() {
  const [text, setText] = useState('');

  return <input type="text" value={text} onChange={(e) => setText(e.target.value)} />;
}
```

### textRef

```tsx
export default function CtrlElement() {
  const textRef = useRef<HTMLInputElement>(null);

  const textRefVal = textRef.current?.value;
  if (textRefVal) console.log(textRefVal);

  return <input type="text" ref={textRef} />;
}
```

### range

```tsx
export default function CtrlElement() {
  const [range, setRange] = useState(0);

  return <input type="range" min={0} max={100} value={range} onChange={(e) => setRange(Number(e.target.value))} />;
}
```

### radio

```tsx
export default function CtrlElement() {
  const [radio, setRadio] = useState('user');

  return (
    <>
      <span>user</span>
      <input type="radio" name="account" value="user" checked={radio === 'user'} onChange={(e) => setRadio(e.target.value)} />

      <span>admin</span>
      <input type="radio" name="account" value="admin" checked={radio === 'admin'} onChange={(e) => setRadio(e.target.value)} />
    </>
  );
}
```

### check box

```tsx
export default function CtrlElement() {
  const [check, setCheck] = useState(false);

  return (
    <>
      <span>true / false</span>
      <input type="checkbox" checked={check} onChange={(e) => setCheck(e.target.checked)} />
    </>
  );
}
```

### check box 2

```tsx
export default function CtrlElement() {
  const [skills, setSkills] = useState<string[]>([]);

  function handleChange(e: React.ChangeEvent<HTMLInputElement>) {
    const { value, checked } = e.target;

    if (checked) {
      setSkills([...skills, value]);
    } else {
      setSkills(skills.filter((skill) => skill !== value));
    }
  }

  return (
    <>
      <label>
        <input type="checkbox" value="React" onChange={handleChange} />
        React
      </label>

      <label>
        <input type="checkbox" value="Node.js" onChange={handleChange} />
        Node.js
      </label>

      <label>
        <input type="checkbox" value="Python" onChange={handleChange} />
        Python
      </label>
    </>
  );
}
```

### select box

```tsx
export default function CtrlElement() {
  const [selectBox, setSelectBox] = useState('');

  return (
    <select value={selectBox} onChange={(e) => setSelectBox(e.target.value)}>
      <option value="">Select a country</option>
      <option value="iran">Iran</option>
      <option value="germany">Germany</option>
      <option value="canada">Canada</option>
    </select>
  );
}
```
