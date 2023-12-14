$user_name = "boxgates-dung";
$repo_name = "test";
$token     = "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";

function check_ver($user_name, $repo_name, $token)
{
  $curl = curl_init();

  curl_setopt_array($curl, array(
    CURLOPT_URL             => "https://api.github.com/repos/{$user_name}/{$repo_name}/contents/version.txt",
    CURLOPT_RETURNTRANSFER  => true,
    CURLOPT_TIMEOUT         => 30,
    CURLOPT_HTTP_VERSION    => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST   => "GET",
    CURLOPT_HTTPHEADER      => array(
      "cache-control: no-cache",
      "User-Agent: Awesome-Octocat-App",
      // "x-ratelimit-limit: 5000",
      "Authorization: Bearer {$token}",
    ),
  ));

  $response   = curl_exec($curl);
  $err        = curl_error($curl);
  curl_close($curl);
  $response   = json_decode($response, true);

  if ($err || empty($response['content'])) return null;
  $ver        =  base64_decode(str_replace("Cg==", "", $response['content']), false);
  return $ver;
}

print_r(check_ver($user_name, $repo_name, $token));
