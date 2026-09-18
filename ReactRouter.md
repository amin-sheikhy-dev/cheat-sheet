در این فایل تمام مسیرهای برنامه تعریف شده‌اند. AppLayout به‌عنوان لایه‌ی اصلی برنامه در نظر گرفته شده و تمام صفحات داخل Outlet آن رندر می‌شوند. همچنین مسیرهای تو در تو (Nested Routes)، مسیرهای محافظت‌شده (Protected Routes)، صفحه‌ی خطا (errorElement) و صفحه‌ی NotFound نیز در همین فایل تنظیم شده‌اند.

```tsx
import { createBrowserRouter, Navigate, RouterProvider } from 'react-router-dom';

const router = createBrowserRouter([
  {
    // پرنت تمام روت های اپلیکشن حساب میشه AppLayout
    element: <AppLayout />,

    // اصلی میشه element اصلی به وجود بیاد جایگزین element وقتی مشکلی در errorElement
    errorElement: <Error />,

    // رو نشون میده Home هرکدوم از اینا که رندر بشه رو نشون میده ولی اول از همه Outlet بجای AppLayout توی
    children: [
      {
        path: '/',
        element: <Home />,
      },

      {
        path: '/login',
        element: <Login />,
      },

      // این الان تبدیل شده به روت محافظت شده
      {
        path: '/menu',
        element: (
          <ProtectedRoute>
            <Menu />
          </ProtectedRoute>
        ),
      },

      {
        path: '/menu/:id',
        element: (
          <ProtectedRoute>
            <MenuItem />
          </ProtectedRoute>
        ),
      },

      {
        path: '/about',
        element: <About />,
        children: [
          { index: true, element: <Navigate to="/about/you" replace /> },
          { path: '/about/you', element: <AboutYou /> },
          { path: '/about/me', element: <AboutMe /> },
        ],
      },

      {
        path: '*',
        element: <NotFoundPage />,
      },
    ],
  },
]);

export default function App() {
  return <RouterProvider router={router} />;
}
```

کامپوننت AppLayout قالب اصلی برنامه است. در این کامپوننت Header و Footer به‌صورت ثابت نمایش داده می‌شوند و هر صفحه‌ای که از طریق Router انتخاب شود، داخل Outlet بین آن‌ها رندر می‌شود.

```tsx
import { Outlet } from 'react-router-dom';
import Header from './components/UI/header';
import Footer from './components/UI/footer';

export default function AppLayout() {
  return (
    <div>
      <Header />

      <Outlet />

      <Footer />
    </div>
  );
}
```

کامپوننت About یک مسیر تو در تو (Nested Route) است. لینک‌های موجود بین صفحات فرزند جابه‌جا می‌شوند و کامپوننت‌های AboutMe یا AboutYou داخل Outlet این صفحه نمایش داده می‌شوند. همچنین با ورود به آدرس /about، کاربر به‌صورت خودکار به /about/you هدایت می‌شود.

```tsx
import { Link, Outlet } from 'react-router-dom';

export default function About() {
  return (
    <div>
      <h1>'about Component'</h1>

      <div>
        <Link to="/about/me">about me Component❤️</Link>

        <Link to="/about/you">about you Component🌙</Link>
      </div>

      <Outlet />

      <Link to="/about/us">about us Component :D</Link>
    </div>
  );
}
```

### داینامیک روت

```tsx
import { useQuery } from '@tanstack/react-query';
import { fetchMeals } from '../lib/api';
import { Link } from 'react-router-dom';

export default function Menu() {
  const { data, isPending, isError, error } = useQuery({
    queryKey: ['meals'],
    queryFn: fetchMeals,
  });

  if (isPending) {
    return <p>Loading...</p>;
  }

  if (isError) {
    return <p>{error.message}</p>;
  }

  // console.log(data);

  return (
    <div>
      {data.meals?.map((m) => (
        <div key={m.idMeal}>
          <Link to={`/menu/${m.idMeal}`}>{m.strMeal}</Link>
        </div>
      ))}
    </div>
  );
}
```

```tsx
import { useParams } from 'react-router-dom';
import { getMeal } from '../lib/api';

export default function MenuItem() {
  const { id } = useParams();
  const [meal, setMeal] = useState<{ meals: Record<string, string>[] } | null>(null);

  useEffect(() => {
    async function loadMeal() {
      if (!id) return;

      const data = await getMeal(id);

      setMeal(data);
    }

    loadMeal();
  }, [id]);

  // console.log(meal);

  return (
    <div>
      <p>id={id}</p>

      <p>{meal?.meals?.[0].strMeal}</p>
    </div>
  );
}
```
