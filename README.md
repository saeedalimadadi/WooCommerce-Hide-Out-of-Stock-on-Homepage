# WooCommerce Hide Out-of-Stock on Homepage

This code snippet provides a solution to **hide out-of-stock WooCommerce products from your WordPress homepage or front page**. By default, WooCommerce might still display products even if they are out of stock. This modification ensures that only in-stock products are shown on your main website's landing pages, providing a cleaner and more actionable product display for your customers.

## Why hide out-of-stock products on the homepage?

* **Improved User Experience:** Prevents customers from seeing products they cannot purchase, reducing frustration.
* **Cleaner Product Display:** Keeps your homepage focused on available products, enhancing visual appeal and conversion potential.
* **Better Conversion Rates:** By showing only purchasable items, you guide users more effectively towards making a purchase.
* **Streamlined Browse:** Users don't have to filter or scroll past unavailable items.

## Installation

There are two primary methods to implement this code:

### Method 1: As a Standalone Plugin (Recommended)

Creating a small, dedicated plugin is the most robust way to add this functionality. It ensures your modification remains active regardless of theme changes.

1.  Create a new folder named `wc-homepage-stock-filter` inside your WordPress site's `wp-content/plugins/` directory.
2.  Inside this new folder, create a file named `wc-homepage-stock-filter.php`.
3.  Copy and paste the following code into `wc-homepage-stock-filter.php`:

    ```php
    <?php
    /**
     * Plugin Name: WooCommerce Hide Out-of-Stock on Homepage
     * Description: Hides out-of-stock WooCommerce products from the WordPress homepage/front page.
     * Version: 1.0
     * Author: Your Name (Optional)
     * License: GPL-2.0-or-later
     */

    if ( ! defined( 'ABSPATH' ) ) {
        exit; // Exit if accessed directly.
    }

    /**
     * Modifies the main query on the homepage/front page to exclude out-of-stock products.
     *
     * @param WP_Query $q The WP_Query instance.
     */
    function custom_pre_get_posts_query( $q ) {
        // Ensure it's not in the admin, and it's the main query.
        if ( ! is_admin() && $q->is_main_query() ) {
            // Apply only to the homepage or front page.
            if ( is_home() || is_front_page() ) {
                $meta_query = $q->get( 'meta_query' );

                // Ensure meta_query is an array.
                if ( ! is_array( $meta_query ) ) {
                    $meta_query = [];
                }

                // Add meta condition to exclude out-of-stock products.
                $meta_query[] = array(
                    'key'     => '_stock_status',
                    'value'   => 'outofstock',
                    'compare' => 'NOT IN',
                );

                $q->set( 'meta_query', $meta_query );
            }
        }
    }
    add_action( 'pre_get_posts', 'custom_pre_get_posts_query' );
    ```

4.  Go to your WordPress admin dashboard, navigate to **"Plugins"**, and **activate** the "WooCommerce Hide Out-of-Stock on Homepage" plugin.

### Method 2: Adding to your Theme's functions.php File

You can add this code directly to your active theme's `functions.php` file. **Before doing so, it's highly recommended to back up your `functions.php` file.**

1.  Navigate to `wp-content/themes/YourThemeName/` (replace `YourThemeName` with the actual name of your active theme).
2.  Open the `functions.php` file.
3.  Add the following code to the end of the file (before the closing `?>` tag, if one exists):

    ```php
    /**
     * Modifies the main query on the homepage/front page to exclude out-of-stock products.
     *
     * @param WP_Query $q The WP_Query instance.
     */
    function custom_pre_get_posts_query( $q ) {
        // Ensure it's not in the admin, and it's the main query.
        if ( ! is_admin() && $q->is_main_query() ) {
            // Apply only to the homepage or front page.
            if ( is_home() || is_front_page() ) {
                $meta_query = $q->get( 'meta_query' );

                // Ensure meta_query is an array.
                if ( ! is_array( $meta_query ) ) {
                    $meta_query = [];
                }

                // Add meta condition to exclude out-of-stock products.
                $meta_query[] = array(
                    'key'     => '_stock_status',
                    'value'   => 'outofstock',
                    'compare' => 'NOT IN',
                );

                $q->set( 'meta_query', $meta_query );
            }
        }
    }
    add_action( 'pre_get_posts', 'custom_pre_get_posts_query' );
    ```

## Important Considerations

* **Homepage Only:** This code specifically targets the **homepage** (`is_home()` for blog posts index) and **front page** (`is_front_page()` for static front page). It will **not** affect shop pages, category pages, or other archive pages. If you want to hide out-of-stock products across your entire shop, WooCommerce has a built-in option for this under **WooCommerce > Settings > Products > Inventory > Out of Stock visibility**.
* **Query Modification:** This code modifies the main WordPress query. While generally safe, always test thoroughly after implementation, especially if you have other plugins that also modify queries.

## Contributing

Contributions are welcome! If you have suggestions or improvements for this code, feel free to open a "Pull Request" or report an "Issue."

## License

This project is licensed under the GPL-2.0-or-later License.

# پنهان کردن محصولات ناموجود ووکامرس در صفحه اصلی

این قطعه کد راه حلی برای **پنهان کردن محصولات ناموجود (Out-of-Stock) ووکامرس از صفحه اصلی یا صفحه نخست وب‌سایت وردپرس شما** ارائه می‌دهد. به طور پیش‌فرض، ووکامرس ممکن است محصولات را حتی اگر ناموجود باشند، نمایش دهد. این تغییر تضمین می‌کند که فقط محصولات موجود در انبار در صفحات اصلی وب‌سایت شما نمایش داده شوند و نمایش محصولی تمیزتر و کاربردی‌تر را برای مشتریان شما فراهم می‌کند.

## چرا محصولات ناموجود را در صفحه اصلی پنهان کنیم؟

* **بهبود تجربه کاربری:** از دیدن محصولاتی که مشتریان نمی‌توانند خریداری کنند، جلوگیری می‌کند و از ناامیدی آن‌ها می‌کاهد.
* **نمایش محصول تمیزتر:** صفحه اصلی شما را بر روی محصولات موجود متمرکز می‌کند و جذابیت بصری و پتانسیل تبدیل را افزایش می‌دهد.
* **نرخ تبدیل بهتر:** با نمایش تنها اقلام قابل خرید، کاربران را به طور مؤثرتری به سمت خرید راهنمایی می‌کنید.
* **مرور ساده‌تر:** کاربران مجبور نیستند محصولات ناموجود را فیلتر کنند یا از آن‌ها عبور کنند.

## نصب

برای پیاده‌سازی این کد، دو روش اصلی وجود دارد:

### روش ۱: به عنوان یک افزونه مستقل (توصیه شده)

ایجاد یک افزونه کوچک و اختصاصی، قوی‌ترین راه برای افزودن این قابلیت است. این تضمین می‌کند که کد شما صرف‌نظر از تغییر قالب، فعال باقی بماند.

1.  یک پوشه جدید با نام `wc-homepage-stock-filter` در مسیر `wp-content/plugins/` سایت وردپرسی خود ایجاد کنید.
2.  در داخل این پوشه جدید، یک فایل با نام `wc-homepage-stock-filter.php` ایجاد کنید.
3.  کد زیر را در `wc-homepage-stock-filter.php` کپی و جایگذاری کنید:

    ```php
    <?php
    /**
     * Plugin Name: WooCommerce Hide Out-of-Stock on Homepage
     * Description: Hides out-of-stock WooCommerce products from the WordPress homepage/front page.
     * Version: 1.0
     * Author: Your Name (Optional)
     * License: GPL-2.0-or-later
     */

    if ( ! defined( 'ABSPATH' ) ) {
        exit; // Exit if accessed directly.
    }

    /**
     * Modifies the main query on the homepage/front page to exclude out-of-stock products.
     *
     * @param WP_Query $q The WP_Query instance.
     */
    function custom_pre_get_posts_query( $q ) {
        // اطمینان از اینکه در بخش مدیریت نیست و یک کوئری اصلی است.
        if ( ! is_admin() && $q->is_main_query() ) {
            // فقط در صفحه اصلی یا صفحه نخست اعمال شود.
            if ( is_home() || is_front_page() ) {
                $meta_query = $q->get( 'meta_query' );

                // اطمینان از اینکه meta_query یک آرایه است.
                if ( ! is_array( $meta_query ) ) {
                    $meta_query = [];
                }

                // اضافه کردن شرط متا برای حذف محصولات ناموجود.
                $meta_query[] = array(
                    'key'     => '_stock_status',
                    'value'   => 'outofstock',
                    'compare' => 'NOT IN',
                );

                $q->set( 'meta_query', $meta_query );
            }
        }
    }
    add_action( 'pre_get_posts', 'custom_pre_get_posts_query' );
    ```

4.  وارد پنل مدیریت وردپرس خود شوید، به بخش **"افزونه‌ها"** بروید و افزونه **"پنهان کردن محصولات ناموجود ووکامرس در صفحه اصلی"** را **فعال کنید**.

### روش ۲: اضافه کردن به فایل functions.php قالب شما

می‌توانید این کد را مستقیماً به فایل `functions.php` قالب فعال خود اضافه کنید. **پیشنهاد اکید می‌شود قبل از انجام این کار، از فایل `functions.php` خود یک پشتیبان (backup) تهیه کنید.**

1.  به مسیر `wp-content/themes/YourThemeName/` بروید (به جای `YourThemeName` نام واقعی قالب فعال خود را قرار دهید).
2.  فایل `functions.php` را باز کنید.
3.  کد زیر را به انتهای فایل (قبل از تگ بستن `?>`، در صورت وجود) اضافه کنید:

    ```php
    /**
     * Modifies the main query on the homepage/front page to exclude out-of-stock products.
     *
     * @param WP_Query $q The WP_Query instance.
     */
    function custom_pre_get_posts_query( $q ) {
        // اطمینان از اینکه در بخش مدیریت نیست و یک کوئری اصلی است.
        if ( ! is_admin() && $q->is_main_query() ) {
            // فقط در صفحه اصلی یا صفحه نخست اعمال شود.
            if ( is_home() || is_front_page() ) {
                $meta_query = $q->get( 'meta_query' );

                // اطمینان از اینکه meta_query یک آرایه است.
                if ( ! is_array( $meta_query ) ) {
                    $meta_query = [];
                }

                // اضافه کردن شرط متا برای حذف محصولات ناموجود.
                $meta_query[] = array(
                    'key'     => '_stock_status',
                    'value'   => 'outofstock',
                    'compare' => 'NOT IN',
                );

                $q->set( 'meta_query', $meta_query );
            }
        }
    }
    add_action( 'pre_get_posts', 'custom_pre_get_posts_query' );
    ```

## ملاحظات مهم

* **فقط صفحه اصلی:** این کد به طور خاص **صفحه اصلی** (`is_home()` برای فهرست نوشته‌های وبلاگ) و **صفحه نخست** (`is_front_page()` برای صفحه نخست ثابت) را هدف قرار می‌دهد. این کد بر صفحات فروشگاه، صفحات دسته‌بندی یا سایر صفحات آرشیو **تأثیری نخواهد گذاشت**. اگر می‌خواهید محصولات ناموجود را در سراسر فروشگاه خود پنهان کنید، ووکامرس یک گزینه داخلی برای این کار در مسیر **ووکامرس > تنظیمات > محصولات > موجودی > قابلیت مشاهده محصولات ناموجود در انبار** دارد.
* **تغییر کوئری:** این کد کوئری اصلی وردپرس را تغییر می‌دهد. اگرچه به طور کلی ایمن است، همیشه پس از پیاده‌سازی به طور کامل آزمایش کنید، به خصوص اگر افزونه‌های دیگری دارید که آن‌ها نیز کوئری‌ها را تغییر می‌دهند.

## مشارکت (Contributing)

مشارکت شما خوشایند است! اگر پیشنهاد یا بهبودهایی برای این کد دارید، می‌توانید یک "Pull Request" ایجاد کنید یا "Issue" جدیدی را گزارش دهید.

## مجوز (License)

این پروژه تحت مجوز GPL-2.0-or-later منتشر شده است.
