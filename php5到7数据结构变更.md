PHP7
typedef struct {
	zend_string *s;
	size_t a;
} smart_str;

struct _zend_string {
	zend_refcounted_h gc;
	zend_ulong        h;                /* hash value */
	size_t            len;
	char              val[1];
};

PHP5
typedef struct {
	char *c;
	size_t len;
	size_t a;
} smart_str;

PHP7
ZEND_API int add_next_index_long(zval *arg, zend_long n) /* {{{ */
{
	zval tmp;

	ZVAL_LONG(&tmp, n);
	return zend_hash_next_index_insert(Z_ARRVAL_P(arg), &tmp) ? SUCCESS : FAILURE;
}

PHP5
ZEND_API int add_next_index_long(zval *arg, long n) /* {{{ */
{
	zval *tmp;

	MAKE_STD_ZVAL(tmp);
	ZVAL_LONG(tmp, n);

	return zend_hash_next_index_insert(Z_ARRVAL_P(arg), &tmp, sizeof(zval *), NULL);
}