.. -*- coding: utf-8 -*-
.. URL: https://docs.docker.com/engine/reference/commandline/images/
.. SOURCE: https://github.com/docker/docker/blob/master/docs/reference/commandline/images.md
   doc version: 1.11
      https://github.com/docker/docker/commits/master/docs/reference/commandline/images.md
.. check date: 2016/04/26
.. Commits on Apr 8, 2016 b83e9df7609e705a85545a8a63db81ef73d85b0e
.. -------------------------------------------------------------------

.. images

=======================================
images
=======================================

.. sidebar:: 目次

   .. contents:: 
       :depth: 3
       :local:

.. code-block:: bash

   Usage: docker images [OPTIONS] [REPOSITORY[:TAG]]
   
   List images
   
     -a, --all=false      Show all images (default hides intermediate images)
     --digests=false      Show digests
     -f, --filter=[]      Filter output based on conditions provided
     --help               Print usage
     --no-trunc=false     Don't truncate output
     -q, --quiet=false    Only show numeric IDs

.. The default docker images will show all top level images, their repository and tags, and their size.

標準の ``docker image`` は全てのトップ・レベルのイメージと、リポジトリ・タグ・容量を表示します。

.. Docker images have intermediate layers that increase reusability, decrease disk usage, and speed up docker build by allowing each step to be cached. These intermediate layers are not shown by default.

Docker イメージは中間レイヤ（intermediate layer）を持っています。これは再利用性を高め、ディスク容量を減らし、 ``docker build`` は各ステップがキャッシュされるので速度を向上します。デフォルトでは、これらの中間レイヤは表示されません。

.. The SIZE is the cumulative space taken up by the image and all its parent images. This is also the disk space used by the contents of the Tar file created when you docker save an image.

``SIZE`` （容量）は、イメージと全ての親イメージの累積した領域です。また、 ``docker save`` でイメージを作成していた場合、 Tar ファイルの内容に含まれるディスク容量です。

.. An image will be listed more than once if it has multiple repository names or tags. This single image (identifiable by its matching IMAGE ID) uses up the SIZE listed only once.

イメージ一覧では、複数のリポジトリ名やタグが表示されます。イメージ（ ``IMAGE ID`` が一致するもの ）ごとの ``SIZE`` が複数表示されますが、実際に対象としている容量は１つだけです。

.. Listing the most recently created images

.. _listing-the-most-recently-created-images:

最近作成されたイメージから一覧表示
--------------------------------------------------

.. code-block:: bash

   $ docker images
   REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE
   <none>                    <none>              77af4d6b9913        19 hours ago        1.089 GB
   committ                   latest              b6fa739cedf5        19 hours ago        1.089 GB
   <none>                    <none>              78a85c484f71        19 hours ago        1.089 GB
   docker                    latest              30557a29d5ab        20 hours ago        1.089 GB
   <none>                    <none>              5ed6274db6ce        24 hours ago        1.089 GB
   postgres                  9                   746b819f315e        4 days ago          213.4 MB
   postgres                  9.3                 746b819f315e        4 days ago          213.4 MB
   postgres                  9.3.5               746b819f315e        4 days ago          213.4 MB
   postgres                  latest              746b819f315e        4 days ago          213.4 MB


.. Listing images by name and tag

.. _listing-images-by-name-and-tag:

イメージ名とタグで一覧表示
------------------------------

.. The docker images command takes an optional [REPOSITORY[:TAG]] argument that restricts the list to images that match the argument. If you specify REPOSITORYbut no TAG, the docker images command lists all images in the given repository.

``docker images`` コマンドは、オプションで ``[リポジトリ[:タグ]]`` を指定できます。これはイメージ一覧から条件が一致するものだけ表示します。 ``リポジトリ`` は ``タグ`` を指定しなくても使えるので、 ``docker images`` で対象となるリポジトリの全イメージのみ表示します。

.. For example, to list all images in the “java” repository, run this command :

例えば、「java」リポジトリにあるイメージを表示するには、次のコマンドを実行します。

.. code-block:: bash

   $ docker images java
   REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
   java                8                   308e519aac60        6 days ago          824.5 MB
   java                7                   493d82594c15        3 months ago        656.3 MB
   java                latest              2711b1d6f3aa        5 months ago        603.9 MB

.. The [REPOSITORY[:TAG]] value must be an “exact match”. This means that, for example, docker images jav does not match the image java.

``[リポジトリ[:タグ]]`` 値は「完全一致」の必要があります。つまり、 ``docker images jav`` は ``java`` イメージに一致しません。

.. If both REPOSITORY and TAG are provided, only images matching that repository and tag are listed. To find all local images in the “java” repository with tag “8” you can use:

``リポジトリ`` と ``タグ`` の両方が指定された場合は、リポジトリとタグが一致するイメージのみ表示します。ローカルにある「java」リポジトリで、タグが「8」のイメージを表示するには、次のように実行します。

.. code-block:: bash

   $ docker images java:8
   REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
   java                8                   308e519aac60        6 days ago          824.5 MB

.. If nothing matches REPOSITORY[:TAG], the list is empty.

もし一致する ``[リポジトリ[:タグ]]`` がなければ、何も表示しません。

.. code-block:: bash

   $ docker images java:0
   REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE

.. Listing the full length image IDs

.. _listing-the-full-length-image-ids:

長いイメージ ID を全て表示
==============================

.. code-block:: bash

   $ docker images --no-trunc
   REPOSITORY                    TAG                 IMAGE ID                                                                  CREATED             SIZE
   <none>                        <none>              sha256:77af4d6b9913e693e8d0b4b294fa62ade6054e6b2f1ffb617ac955dd63fb0182   19 hours ago        1.089 GB
   committest                    latest              sha256:b6fa739cedf5ea12a620a439402b6004d057da800f91c7524b5086a5e4749c9f   19 hours ago        1.089 GB
   <none>                        <none>              sha256:78a85c484f71509adeaace20e72e941f6bdd2b25b4c75da8693efd9f61a37921   19 hours ago        1.089 GB
   docker                        latest              sha256:30557a29d5abc51e5f1d5b472e79b7e296f595abcf19fe6b9199dbbc809c6ff4   20 hours ago        1.089 GB
   <none>                        <none>              sha256:0124422dd9f9cf7ef15c0617cda3931ee68346455441d66ab8bdc5b05e9fdce5   20 hours ago        1.089 GB
   <none>                        <none>              sha256:18ad6fad340262ac2a636efd98a6d1f0ea775ae3d45240d3418466495a19a81b   22 hours ago        1.082 GB
   <none>                        <none>              sha256:f9f1e26352f0a3ba6a0ff68167559f64f3e21ff7ada60366e2d44a04befd1d3a   23 hours ago        1.089 GB
   tryout                        latest              sha256:2629d1fa0b81b222fca63371ca16cbf6a0772d07759ff80e8d1369b926940074   23 hours ago        131.5 MB
   <none>                        <none>              sha256:5ed6274db6ceb2397844896966ea239290555e74ef307030ebb01ff91b1914df   24 hours ago        1.089 GB

.. Listing image digests

.. _listing-image-digest:

イメージの digest 値を表示
==============================

.. Images that use the v2 or later format have a content-addressable identifier called a digest. As long as the input used to generate the image is unchanged, the digest value is predictable. To list image digest values, use the --digests flag:

v2 移行の形式を使うイメージには、 ``digest`` と呼ばれる識別子が割り振られます。イメージ生成後に変更が加えられなければ、digest 値は変更されていないと考えられます。全ての digest 値を表示するには、 ``--digests`` フラグを使います。

.. code-block:: bash

   $ docker images --digests
   REPOSITORY                         TAG                 DIGEST                                                                    IMAGE ID            CREATED             SIZE
   localhost:5000/test/busybox        <none>              sha256:cbbf2f9a99b47fc460d422812b6a5adff7dfee951d8fa2e4a98caa0382cfbdbf   4986bf8c1536        9 weeks ago         2.43 MB

.. When pushing or pulling to a 2.0 registry, the push or pull command output includes the image digest. You can pull using a digest value. You can also reference by digest in create, run, and rmi commands, as well as the FROM image reference in a Dockerfile.

2.0 レジストリに対して送信（push） や取得（pull ）する場合は、 ``push`` と ``pull`` コマンドの出力にイメージの digest も含まれます。digest 値を使っても ``pull`` できます。digest 値が使えるのは ``create`` 、 ``run`` 、 ``rmi`` の各コマンドと、 Dockerfile のイメージを参照する ``FROM`` でも同様です。

.. Filtering

.. _images-filtering:

フィルタリング
====================

.. The filtering flag (-f or --filter) format is of “key=value”. If there is more than one filter, then pass multiple flags (e.g., --filter "foo=bar" --filter "bif=baz")

フィルタリング・フラグ（ ``-f`` と ``--filter`` ）の形式は「key=value」です。複数のファイル多を使う時は、複数のフラグを使います（例： ``--filter "foo=bar" --filter "bif=baz"`` ）。

.. The currently supported filters are:

現在サポートされているフィルタ：

..    dangling (boolean - true or false)
    label (label=<key> or label=<key>=<value>)

* ダングリング（宙ぶらりんな状態）なイメージ （ブール値： true か false ）
* ラベル（ ``label=<key>`` か ``lavel=<key>=<value>`` ）

.. Untagged images (dangling)

タグ付けされていないイメージ（dangling）
--------------------------------------------------

.. code-block:: bash

   $ docker images --filter "dangling=true"
   
   REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
   <none>              <none>              8abc22fbb042        4 weeks ago         0 B
   <none>              <none>              48e5f45168b9        4 weeks ago         2.489 MB
   <none>              <none>              bf747efa0e2f        4 weeks ago         0 B
   <none>              <none>              980fe10e5736        12 weeks ago        101.4 MB
   <none>              <none>              dea752e4e117        12 weeks ago        101.4 MB
   <none>              <none>              511136ea3c5a        8 months ago        0 B

.. This will display untagged images, that are the leaves of the images tree (not intermediary layers). These images occur when a new build of an image takes the repo:tag away from the image ID, leaving it untagged. A warning will be issued if trying to remove an image when a container is presently using it. By having this flag it allows for batch cleanup.

これはタグ付けされておらず、イメージ・ツリーから離れた（中間レイヤではない）イメージを表示します。これらのタグがないイメージは、イメージを使って新しく構築しようとしても ``リポジトリ:タグ`` の形式が利用できないため、その場合はイメージ ID を使います。コンテナが利用中であれば、イメージを削除しようとしても警告が表示されます。バッチ処理でクリーンアップするときに、このフラグが使えます。

.. Ready for use by docker rmi ..., like:

``docker rmi`` に対応するには：

.. code-block:: bash

   $ docker rmi $(docker images -f "dangling=true" -q)
   
   8abc22fbb042
   48e5f45168b9
   bf747efa0e2f
   980fe10e5736
   dea752e4e117
   511136ea3c5a

.. NOTE: Docker will warn you if any containers exist that are using these untagged images.

.. note::

   タグ付けされていないイメージでも、何らかのコンテナが使用中であれば Docker は警告を表示します。

.. Labeled images

.. _labeled-images:

ラベル付けされたイメージ
------------------------------

.. The label filter matches images based on the presence of a label alone or a label and a value.

``label`` フィルタは、 ``label`` そのものが一致するイメージか、ラベルの値に一致する場合に表示します。

.. The following filter matches images with the com.example.version label regardless of its value.

次のフィルタは ``com.example.version`` に一致するラベルだけでなく、その値にも適用されます。

.. code-block:: bash

   $ docker images --filter "label=com.example.version"
   
   REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
   match-me-1          latest              eeae25ada2aa        About a minute ago   188.3 MB
   match-me-2          latest              eeae25ada2aa        About a minute ago   188.3 MB

.. The following filter matches images with the com.example.version label with the 1.0 value.

次のフィルタは ``com.example.version`` ラベルと ``1.0`` 値に一致するイメージを表示します。

.. code-block:: bash

   $ docker images --filter "label=com.example.version=1.0"
   REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
   match-me            latest              eeae25ada2aa        About a minute ago   188.3 MB

.. In this example, with the 0.1 value, it returns an empty set because no matches were found.

次の例は、 ``0.1`` 値を持つものをフィルタしますが、一致するものが無かったため、何も表示されません。

.. code-block:: bash

   $ docker images --filter "label=com.example.version=0.1"
   REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE

.. seealso:: 

   images
      https://docs.docker.com/engine/reference/commandline/images/


