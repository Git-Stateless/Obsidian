# PHP文件包含常见函数
1. include。包含并运行指定文件，include在出错时会产生警告（E_WARNING），脚本会继续运行；
2.  include_once。在脚本运行期间包含并运行指定文件，该函数和include函数类似，两者唯一的区别是：使用该函数时，PHP会检查指定文件是否已经被包含过，如果是则不会再次包含；
3.  require。包含并运行指定文件，require在出错时会产生警告（E_COMPRILE_ERROR）级别的错误，导致脚本终止；
4.  require_once。它和require函数完全相同。两者唯一的区别是：使用该函数时，PHP会检查指定文件是否已经被包含过，如果是则不会再次包含；