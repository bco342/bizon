# Как почти любую задачу, так и этот метод можно реализовать несколькими способами

Реализация с формированием массивов для поиска и замены через array_map()
```
public function get_api_path(array $array, string $template) : string
{
    $search = array_map(fn($key) => "%{$key}%", array_keys($array));
    $replace = array_map(fn($value) => rawurlencode((string)$value), $array);

    return str_replace($search, $replace, $template);
}
```

Реализация через регулярные выражения
```
public function get_api_path(array $array, string $template) : string
{
    return preg_replace_callback('~%(\w+)%~', function ($matches) use ($array) {
        $key = $matches[1];
        return isset($array[$key]) ? rawurlencode((string)$array[$key]) : $matches[0];
    }, $template);
}
```

Но мне больше нравится вариант с foreach(), как самый человекочитаемый )
